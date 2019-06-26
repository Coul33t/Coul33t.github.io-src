Title: What are Markov Chains, and how to use them to generate sentences?
Date: 2019-04-09 15:00
Modified: 2019-04-09 15:00
Tags: Algorithm Python Tutorial
Category: Algorithm Python
Slug: What-are-Markov-Chains-and-how-to-use-them-to-generate-sentences
Authors: Quentin Couland
Summary: A quick explanation about Markov Chains, and how to use them to do fun stuff

Henlo ma friands!

Some times ago, I developped a [bot](https://twitter.com/globabot) that allows me to generate sentences from gathered tweets. The code of the bot is available [here](https://github.com/Coul33t/twitter_bot), and here's an example of an input, and an output :

* Input : `py main.py -a Coul33t -n -p 20 -m 3 -r` (You can read the bot's Github page to understand what does the different parameters do)
* Output : `Je donne 1h de la musique en ligne dans les neurones de l'homme et la détresse des gens. I now have a l w a y s`
(`I give 1 hour of online music in the man's neuron and people's distress. I now have a l w a y s` )

As you can see, it does not make a lot of sense, but the sentence seems to " have something " coherent in it. The output is generated from my tweets, by " mixing " them together, in a kinda-coherent way.

It uses Markov Chains (I developped [the Markov Chains library](https://github.com/Coul33t/markold) used in the bot by myself, so it's probably messy and not optimal at all) to achieve this. Now, there are some way better ways to do it nowadays (mainly with the famous Neural Networks), but Markov Chains have the advantage of being easily understandable, and kinda easy to implement.

## What is a Markov Chain?

I *could* put the Wikipedia definition here, but I think that knowing that
>a Markov chain is a stochastic model describing a sequence of possible events in which the probability of each event depends only on the state attained in the previous event

wouldn't be very helpful. Well, actually, some parts of it are interesting, mainly this one:
>the probability of each event depends only on the state attained in the previous event

From this part of the sentence, we can understand that a Markov chain has __events__ and __probabilities__ that are conjointly used: you have a probability to go from an event to another.

Let's take an example of such a system. I will describe my typical evening/night. For the sake of simplicity, I will only use a small subset of what I usually do.
When I **_come home_**, I **_wash my hands_**, then I usually sit at my desk, **_playing vidayagames_**. But sometimes, I **_play the guitar_** or **_the piano_** or **_the bass_**, depending on my mood. Then, if it isn't too late, I go back **_playing VIDAYAGUMZ_**, until I decide it's time to **_go to sleep_**. Let put some labels on those activities:

* H : coming home
* W : washing hands
* V : playing videogames
* I : playing an instrument (either the guitar, the piano or the bass)
* S : going to sleep

Ok, we got our __events__. Now, when I come home, I usually play videogames before playing any instruments. So there a higher __probability__ that I'll play vuduogumz than an instrument, roughly 75%/25%. When I'm playing videogames, I usually play for a few hours before doing something else. When I play an instrument, I usually play for one hour (sometimes different instruments in this hour). If we decide that we want to represent the chances to switch from an activity to another every hour, we can build an array of probabilities for each event pair of __events__ we have. The first column represent the current event (__X__), the first row represent the next event (__Y__), and the value at the crossing for a specific row and a specific column is the probability to go to __Y__ while we're in __X__:

![The probability matrix.]({attach}img/example_table.png)

(I kept the 0 values blank so that it wouldn't be too hard to read)

The first row represent the probability to do the __Y__ activity after **_coming home (H)_**. Since I said that I always **_wash my hands (W)_** when I **_come home (H)_**, the only probability that is higher than 0 is the **_washing hands (W)_** one. When I'm **_playing videogames (V)_**, I either keep doing it for another hour, switch to **_playing an instrument (I)_** or **_go to sleep (S)_**, which correspond to the third row.

We now have our __probabilities__. Indeed, in this table, since we represented the probability of switching activities every hour, it would mean that I wait one hour at home before washing my hands, then I wait one more hour before deciding to do something. This is unrealistic, but the example still stands nonetheless.

So, we have our __events__, we have our __probabilities__... Do we have a Markov Chain? Well, __yes__! Here is this a visualisation of this example:
![The Markov Chain illustrated.]({attach}img/markov_chain_example.png)

## How does the concept of Markov Chain translates to sentences generation?

That's nice and all, but we still haven't talked about sentences generations. Now that you know what Markov Chains are, the rest is kinda easy to understand. As I said earlier, my bot first gather tweets from a person. I'm using tweets, but it could be any kind of input (for example, all the sentences from a book, a conversation between two people, etc.), as long as it's sentences.

We will use another example to explain how it works. Let say that we gathered these tweets:

* *I am stupid*
* *I shat myself*
* *You are stupid and I don't like you*
* *You shat like I told you*

These sentences are __stupid__, but it's not important. If we want that our generated sentences kinda look that these ones, we have to find a way to reproduce the way they are written. Problem is, we don't want to implement grammatical rules, basic syntax, etc. into our system: this would be very complicated and long. We want to keep it __simple__. A simple way to emulate the way these sentences are constructed is to, well... Use Markov Chains. Our __events__ are the sentences __words__, and our __probabilities__ to go from an event (a word) __X__ to another event (another word) __Y__ are the occurence of such transition in the sentences above. For example, the word " am " is only followed by the word " stupid ", so the probability to go from " am " to " stupid " is 1. The " I " can be followed by " am " (1 time), " shat " (1 time), " don't " (1 time) and " told " (1 time), so the probability to from " I " to each of these words will be 0.25 for each. Here is the corresponding table:

![The probability matrix corresponding to the 4 sentences.]({attach}img/sentences_table.png)

As you can see, we added " BEGIN " and " END " as " words ". These just mark the beginning and the end of the sentences. It helps keeping track of which words can be at the beginning of a sentence, and which words __can__ end a sentence. Here, the sentence can start with either " I " or " You ", and can end with " stupid ", " myself " and " you ".

From here, we have everything we need to code our sentence generator! You can represent the data as the array above, or as objects (each objects is one word, and a list of possible next words with their probability, for example) if you prefer. The algorithm isn't hard to find:

1. Gather sentences from any source you want
2. Compute the probability matrix (as shown above)
3. Choose a word to begin the sentence (takr all words that follow the BEGIN keyword)
4. Randomly choose the next word from the list of the current word's probabilities
5. Stop when the END keyword is chosen

That's it. The algorithm is very simple. From the four sentences above, these are some that could be generated:

* *I am stupid and I don't like you*
* *You shat like you are stupid*
* *I am stupid and I don't like you are stupid*
* *I shat myself*
* *I am stupid and I am stupid and I am stupid and I am stupid and I am stupid and I am stupid and I am stupid and I am stupid...*

As you can see, the sentences aren't always coherent. Worse, you can be stuck in an infinite loop, like in the last example (it's like when you use the suggested words when typing on a smartphone's keyboard, you can eventually be stuck in an infinite loop repeating the same few words again and again). You should include another stop condition in the fifth step of the algorithm, to avoid these cases (although you should end up getting out of the loop). You can also have output sentences that are strictly indentic to the input ones. This one is pretty simple to eliminate: just get rid off the output sentences that are equals to the input ones.

## Are we done yet?

Yes and No.

See, this example works, but it's hard to generate something coherent. One of the problems is that we only used 4 sentences to construct our probability matrix. In real cases, you want a lot of input sentences (so that you can generate a lot of different sentences). Another problem is about the algorithm: it only looks for the next word every time, to compute the probability matrix. In some sentences, it is acceptable to have the word __Y__ after the word __X__, while it is not in other sentences. In every langages (that I know), rules have to be followed to make coherent sentences. We aren't looking into adding these rules into the algorithm to improve it.
A simpler way to have better sentences is to compute the probability for __n__ word to follow the word __X__. The bigger the __n__ value, the more coherent the sentences will be, but there will be less variation in the output sentences (comparing to the original ones). In our case, we used __n = 1__. If we used __n = 2__, our matrix would look like this:

![The probability matrix corresponding to n = 2.]({attach}img/n_2_table.png)

(I did NOT put the whole matrix, too big and messy)

While this limits the range of possible sentences, it generates much " realistic " ones. But be careful about the value of __n__: the higher it goes, the more input sentences you need.

I hope this small explaination about Markov Chains and how to generate sentences with them was clear and useful to you. Cheers!