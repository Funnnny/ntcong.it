+++
date = "2014-02-07T17:36:17+07:00"
title = "Stripe-CTF Level 1 with Golang"
author = "Cong Nguyen"
authorlink = "http://ntcong.it"
authorfacebook = "http://facebook.com/ntcong.it"
authorgoogleplus = "https://plus.google.com/+CongNguyen"
Tags = ["development", "go"]
Categories = ["development", "go"]
thumbnail = "/img/stripe.png"
+++

This year's Stripe-CTF brings some interesting things to the table: Cryptocurrency. I've already known about cryptocurrency and Bitcoin in general, but it's amazing how Bitcoin relates to Git.

The level is here:&nbsp;https://stripe-ctf.com/levels/1

Basically, like Bitcoin, you have to find a block, in this case is a git commit. The commit must have its hash lower than the target difficulty, specified in the file difficulty.txt. You will compete against a bot, and you have to find and submit a block/commit before it does.

As I'm learning Golang, I wrote my miner using it, and managed to bring to 1MHash/s. Some people are going higher, specifically those using OpenCL/GPU to mine.

### Basic

The first version of my code is very simple. First it will add myself a Gitcoin to the file "LEDGER.txt", get the difficulty, and get the required parameters to build the commit. Those tasks will only do once per block found, so any code will work. 

{{% highlight go %}}
    diff = make([]byte, 0)
    diffString := strings.Trim(doExec("cat", "difficulty.txt"), "\n")
    for i := 0; i < len(diffString)/2; i++ {
         n, _ := strconv.ParseInt(diffString[i*2:i*2+2], 16, 0)
         diff = append(diff, byte(n))
    }
    tree = strings.Trim(doExec("git", "write-tree"), "\n")
    parent = strings.Trim(doExec("git", "rev-parse", "HEAD"), "\n")
    timestamp = strings.Trim(doExec("date", "+%s"), "\n")
{{% /highlight %}}

I calculate diff as a byte array because SHA1 library returned the hash as an array, so I don't have to convert SHA1 everytime; doExec is just a function to run the command and return console output as a string. The commit hash can be calculate by SHA1 the string "commit [len_commit]\0[commit_string]". The commit string can be built by this code:  

{{% highlight go %}}
    baseCommit = fmt.Sprintf("tree %s\nparent %s\nauthor CTF user &lt;%s@stripe-ctf.com&gt; %s +0000\ncommitter CTF user &lt;%s@stripe-ctf.com&gt; %s +0000\n\nFu[4]ny got a Gitcoin\nnonce 1", tree, parent, username, timestamp, username, timestamp)
{{% /highlight %}}

You should already notice the last "nonce 1" in the commit string, I will replace it with "nonce 2" and so on until I find a commit with hash lower than difficulty. It's surprisingly simple:


{{% highlight go %}}
    func getSHA1(n int) [20]byte {
     return sha1.Sum([]byte(baseCommitHash <complete id="goog_259276313">+ </complete>"\n"))
    }
    func isHashValid(hash [20]byte) bool {
     for i, char := range hash {
      if i >= len(diff) {
       return false
      }
      if char > diff[i] {
       return false
      }
      if char < diff[i] {
       return true
      }
     }
     return false
    }
{{% /highlight %}}

After finding a coin, I'll write the commit and push it, I was lazy and just pipe everything to command line. It works but maybe a little slow.  

{{% highlight go %}}
    func pushResult(result chan int, done chan bool) {
        var n int
        for {
            n = &lt;-result
            sem &lt;- 1  // Semaphore git command so only one git command at a time
            c1 := exec.Command("echo", getCommit(n, false))
            c2 := exec.Command("git", "hash-object", "-t", "commit", "--stdin", "-w")
            var err error
            c2.Stdin, err = c1.StdoutPipe()
            if err != nil {
                panic(err)
            }
            c2.Stdout = os.Stdout
            _ = c2.Start()
            _ = c1.Run()
            _ = c2.Wait()
            &lt;-sem
            doExec("git", "reset", "--hard", fmt.Sprintf("%x", getCommitSHA(n)))
            res := doExec("git", "push")
            if res != "error" {
                // done &lt;- true
                // do not stop, just wait for a restart
                time.Sleep(10 * time.Second)
            } else {
                log.Println("Error when push, waiting for miner to restart")
                time.Sleep(10 * time.Second)
            }
        }
    }
{{% /highlight %}}

Then I just loop nonce until I found a coin. I use a result channel to communicate between miner thread and pushResult thread, and a done channel to stop when I find a commit. The bot takes about 10 mins to find a block, with just this version I managed to beat it.

### Optimize further

After finishing the task, I can join a Gitcoin instance with all players in Stripe-CTF. I knew that I have to optimize it because I can push it only to 200kH/s

Fortunately, Go has profiling built-in, so I can easily point out that, most of the time my program was waiting for fmt.Sprintf, because I used it to build the commit message.

So, because the commit is fixed, just the nonce changes, I was able to prebuilt the commit message and the commit hash, just by giving the commit the fixed length and nonce has 16 number. I thought I can create SHA1 with that prebuilt commit and boost the speed of SHA1 function a lot, but I can't, so removing fmt.Sprintf was good for me.

And by competing with more people, I have to monitor new block, and restart my miner with a new blockchain. I built a channel into the miner, and another gorountine to monitor and send stop signal.

Once in a while, the miner will check for stop signal and get out of the loop.

{{% highlight go %}}
    select {
    case msg := &lt;-stop:
        if msg {
            // fmt.Printf("Miner %d stopped! Last i is %d\n", begin, i)
            break jobOuter
        }
    default:
    }
{{% /highlight %}}

The monitor function is pretty straightforward, I have two commit hash, last and current, if last commit has different hash than current commit, I do a git hard reset, and send a stop signal.


{{% highlight go %}}
    func monitor(stop chan bool) {
        var hash, newHash string
        hash = strings.Trim(doExec("git", "rev-parse", "--short", "origin/master"), "\n")
        for {
            doExec("git", "fetch", "origin")
            newHash = strings.Trim(doExec("git", "rev-parse", "--short", "origin/master"), "\n")
            if hash != newHash {
                doExec("git", "reset", "--hard", "origin/master")
                // log.Println("New block found! Reseting miners!")
                stop &lt;- true
                hash = newHash
                time.Sleep(1 * time.Second)
            }
            time.Sleep(1 * time.Second)
        }
    }
{{% /highlight %}}

That's almost be all, things I can/should do better:

1.  I have to calculate SHA1 again everytime I change nonce. I should calculate a base SHA1, and feed nonce for each loop. I can't find anyway to do this now, so I choose to ignore.
2.  Calling git by command line is bad, maybe use a git library. Sometimes a push does matter, so implement it directly with socket might be good (just the push)

I ended the game with 756 point, with several Gitcoins found. It's very hard to find a coin because I only have 1.5MH/s with all my PCs, so I need a lucky moment to get one.
