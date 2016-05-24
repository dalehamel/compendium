# Foreword

The field of Computer Science moves very quickly. Innovation is not only present in the field, but is an absolute necessity for the technology industry that has grown around it. As a result, it can be difficult to stay up-to-date with all of the change that supports the field, and forces it to remain adaptive.

Much effort has been taken to provide resources to lower the barrier to entry. Various organizations, such as [code academy](https://www.codecademy.com/) have developed free curriculums providing tutorials on programming various computer languages. The [raspberry pi foundation](https://www.raspberrypi.org/resources/) has helped to reduce the cost of commodity computing hardware to help bring technology opportunities to those who would otherwise not have had the financial means. Indeed, just a few decades ago a personal computer would have costed thousands of dollars, have required skilled assembly, and been difficult to do much of anything with due to the small community of entheusiasts. Nowadays, what would have been considered a supercomputer by the previous generation of information technologists is available for about [cost of a cup of coffee](https://www.raspberrypi.org/products/pi-zero/), at just over the size of a small candy bar.

With all these programming and hardware resources abound, one resource that still seems missing is a manual of practical knowledge for how computers actually work. While there are text books, university courses, and indeed scattered resources available, there isn't a single accessible, up-to-date body of work available to guide an initiate in this area. The purpose of this publication is to fill that gap, and be that resource. This type of computer system knowledge is quite valuable. Many organizations fervantly pursue the mythical "full stack developer" - someone capable of not only understanding the nuances of the programming behind a high-level application, but also its overall architecture, and the underlying systems that support it.

This sort of systems expertise has traditionally belonged in the hands of system's architects, operations personale, system administrators, or so-called "power users". The contemporary, and perhaps more 'politically correct' terms for these specialists are now titles such as 'site reliability engineers', 'Dev/Ops', and as the progressive 'production engineer'. Within the industry, there has long been a divide between those with development and product experience, so called 'product developers', or 'pure devs', and those with operational and systems knowledge. This division can be unhealthy, inefficient, and indeed dangerous. Without a sufficient lack of understanding of the platform and systems that keep an application running, it can be difficult or impossible for a product developer to create efficient, secure, scalable code. Likewise, without an understanding of the nuances of the application itself, a system administrator can become too detached from a problem domain to be able to optimize it.

Thus, the important of blurring the lines of this divide have real practical advantages. The full-stack developer need not be a myth, or a unicorn. The benefits of full-stack knowledge are far reaching, and this publication aims to facilitate developers wishing to make the journey 'down the stack', as well as to provide a firm foundation to new initiates in the field of information technology. A broad range of topics will be covered, including theoretical and practical aspects of the operation of a computer system, and current 'hot topics' in the field. It is worth noting that [opsschool](www.opsschool.org) is another fantastic resource in this area, though sadly it is quite incomplete, out of date, and largely abandoned.

Please feel free to contribute to this open source publication if you have something to add; be it a correction, addition, or expansion of the material here.

# The stack

The term 'full-stack' is a bit unfortunate in it's ubiquity and ambguity. It's actual meaning has been obscured by inconsistent use, and its role as a buzz-word for recruiting and marketing purposes. To combat this, we'll use the traditional OSI stack model to denote the different layers of abstraction that make up "the stack". By abstraction, we mean that it takes something complicated, and provides a logical simplification of it.

The OSI model provides us with 7 distinct layers of abstraction ranging from the physical world to the virtual world of our application. It is particularly relevant in understanding computer networks, but the terminology it provides is a useful addition to our technical lexicon. It is worth noting up-front that one of the main deficiencies of this model is that it over-simplifies Layer 7 - the application layer. The application layer is can be significantly complicated, and indeed have its own 'stack' with additional layers of abstraction. These will be discussed in greater detail a bit later.

![osi model](img/osi.png) [@wikimedia]

Sufficed to say, we don't need to trouble ourselves with the nuances of each of these abstraction layers just yet. As each layer becomes relevant, we will describe it in greater detail later on. Right now, let's focus on layer 1 and layer 7, the very top and bottom of the OSI stack. The physical layer is the 'real world', ie, the actual physical appliances, devices, and media types that we can see, touch, and hold. Layer 7 is considered the 'application layer', and can be thought of as the purly logical or 'virtual world'. This is the space in which our code runs, and where the true power of information technology is unlocked. As we build our understanding of the stack, we'll develope a model for how these physical and virtual worlds interact.

## Furthur reading

A wonderful resource on this topic is 'Computer Organization and Design: the hardware/software interface'([@cod]), which is a proprietary textbook by a UC Berkely professor on computer achitecture.

# Automata Theory - The theory of computing

Before diving in on the hardware side of computer systems, it's important to build an understanding for the theory, and conceptual model of a computer system. This is a very broad and complicated topic, of which only the surface will be scratched here.

Maybe without realizing it, you probably already understand the gist of what a finite automaton is.

## Flow charts

Everyone has seen a flowchart before. If not, here's a joke flowchart explaining what flowcharts are from the popular nerd website [xkcd](www.xkcd.com):

![flow chart](img/flow_charts.png) [@xkcd]

The idea is pretty simple. A bunch of states are defined as 'boxes', and transitions between them are defined by connecting lines ^[often called 'edges' in formal circles]. At each state in the flow chart, the input (in this case, the answer to the question in the state box) governs which transition to follow, and thus which state to go to next. Using this methodology, it can become fairly simple to break down a complicated sequence of questions into a single, handy, flow chart!

For example, here's real a flowchart that helps to troubleshoot a non-functioning lamp:

![lamp](img/lamp_chart.png) [@wikimedia]

Now, let's imagine that we built a flow chart, and every state asked the question 'yes' or 'no', and we need only answer with 'y' (for yes) or 'n' (for no). As we answered the questions in each state of the flow chart, we could write this down into sequence of letters ^[conventionally called a 'string' of characters - think letters connected together by a piece of string to remember this], ie, 'ynyyny' for 'yes, no, yes, yes, no, yes'. From the starting state then, we can always end up in the same state of the flow chart using this string! If we think of our flow chart as a machine, we could conceptually write down our answers on a long piece of paper, wrapped that up into a cartridge, and treat it like a cassette tape ^[ in case you've never seen one before, a [cassette tapes](https://en.wikipedia.org/wiki/Compact_Cassette) was similar to a VHS, and preceeded CDs, much as VHS preceeded DVDs]. At each state, our flow chart machine would move advance the cassette tape by one character ^[kind of like a [ticker tape](https://en.wikipedia.org/wiki/Ticker_tape) in slow motion], and use that character to transition to the next state.

Our input tape and our flow chart machine are starting to get interesting now. For every one of the final states of the flow chart ^[where the flow chart has solved your problem for you], you could record a tape that gets you to that state, and built up a library of mix tapes that are all of the possible solutions to the problem. These tapes that lead to a solution are special tapes - the input they provide causes our flow chart machine to 'halt', meaning that when we play them back the machine will go throw a series of transitions, until it reaches the final state with the solution, and no longer needs to keep going.

Looking a bit more closely at those tapes, we can see that our machine understands a very simplistic language - strings of 'y' and 'n'. In computer science, we have a special term for a problem where 'yes/no' is a valid answer to a question - a 'boolean' problem. The letters 'y' and 'n' belong to the alphabet we as humans understand, that has 26 letters. However, we're being a bit wasteful here - we're only using 2 letters, and are leaving 24 unused! If we represented our answers with numbers, we could simply use two numbers - '0' for no, and '1' for yes. You might be thinking 'well, what about all those other numbers from 2 to 9'? Those are called decimal numbers ^[deca being the metric prefix for '10', think 'decade'], which is the 'alphabet' of numbers we are used to working with - we count from one until 10, and then we sort of 'roll over', and reuse the same symbols. If we constrain our 'alphabet' of numbers to only the two numbers '0' and '1', can are using a special number system that you might have heard of before called 'binary'! We'll talk a bit more about binary later, but for now all we need to know is that in binary the only valid numbers are '0' and '1', where '0' means 'no', and '1' means 'yes'.

Using this new binary alphabet, we can replace those cumbersome letters on our cassette tapes with more simple '0' and '1'. So our string from earlier 'ynyyny' becomes '101101'. ^[In fact, we need not even write on the tape at all anymore! We could simply punch a hole in the tape to represent a '1', and leave the tape unpunctured to represent a '0'. Not so long ago, this method of punching holes to represent zeros and ones is how [punch cards](https://en.wikipedia.org/wiki/Punched_card) were used to encode input for computers!]

We can actually simplify our flow chart machine quite a bit - we can turn it into a simple table! All we need to do is give each state a name or identifier. By convention, we'll just number them in the order they appear. Now, for each answer '0' or '1', we'll just say what state to go to next. From the lamp troubleshooting example earlier, let's number the different states:

0. Lamp plugged in?
0. Plug in lamp
0. Bulb burned out?
0. Replace bulb
0. Repair lamp

Now we can represent it as a table: ^[Note that "Lamp doesn't work" isn't a real state, as we unconditionally transition to "Lamp plugged in", which is why we have left it out. We have also left out the halting states here (the ones with no transitions), as there is no 'next state' for them.]

|Current state | Input | Next state |
|--------------|-------|------------|
|       0      |   0   |     1      |
|       0      |   1   |     2      |
|       2      |   1   |     3      |
|       2      |   0   |     4      |

This table is a pretty decent representation of our machine. It's not quite so nice to read, but it's a lot easier to draw, and helps us simplify how our machine works - as it reads the tape now, all it needs to do is look what state to transition to given its current state, and the input from the tape. Let's formally call this our 'state transition table', as it tells our machine how to transition between states for each input it might get.

So, to recap, we now have a special machine that solves problem for us - our flow chart machine. It can understand a special language called binary, where '0' corresponds to 'no', and '1' corresponds to 'yes'. Knowing the current state, and the next character read from the input tape, our machine can look up what state to go to text using a state transition table. We can give it a sequence of these, which will take us from our starting state to a state where our problem is solved and the machine 'halts'. What we have done here is built something that now meets the formal definition of a Finite Automaton!

## Finite Automata

Our flowchart machine from the previous example was a machine with a certain number of states, which we consider to be a [Finite State Machine (FSM)](https://en.wikipedia.org/wiki/Finite-state_machine)

In formal terms, there are two types - Deterministic Finite Automatons (DFAs), and Nondeterministic Finite Automatons (NFAs). We won't bother too much with the specifics here, but will point out a couple of the differences:

* Every DFA defines at most one transition for each state, and each possible input character
* Every NFA can be represented as a DFA

In our flow chart machine, we had only two possible input characters - '0' and '1', and at each state we transitioned to exactly one other state. So, what we built was a simple DFA!

Describing our flow chart machine in [formal terms as a DFA](https://en.wikipedia.org/wiki/Deterministic_finite_automaton) ^[don't get thrown off by the math notation here, this is 'theory', which has very little to do with real math. We will use some very basic [set theory](https://en.wikipedia.org/wiki/Set_theory) though]:

* The set of all of the states in our flow chart machines can be called $Q$
* The numbered states from earlier will each be denoted by a $q$ (lowercase), and their number. For instance, state 1 will be $q_1$.
* We'll call the starting state $q_0$ by convention
* Our binary language, containing only '0' and '1', can be called $\Sigma$ ('sigma')
* Our state transition table from earlier, which tells us how to transition between states, can be called $\Delta$ ('delta')
* Our halting states are the ones with no transitions from them, which in our case is $q_3$ and $q_4$
* The set of all halting states will be called $F$
* Inputs into our machine will be strings called $w$, such that $w=a_1a_2 ... a_n$, where $a_i \in \Sigma$ ^[this notation means that each character, repersented by $a$ is part of the alphabet in our language $\Sigma$, in this case, it's a '0' or a '1']

So each of our states $q_0 ... q_n$ is a possible state in our flow chart machine. If we take an arbitrary input string $w$, for each character $a_i$ in $w$, we use the state transition table $\Delta$ to determine which state we should go to next. We'll let $r_0$ be the state that the charactar $a_0$ from $w$ gets us to, using $\Delta$.

Alright, that's enough math. Grab a coffee to wake yourself up, and let's do a practical example!

Let's say that we have a lamp that isn't working. Using our flow chart:

* We go to the first state $q_0$, which asks if the 'Lamp is plugged in?'
* We check the lamp, and determine that yes, which adds the character $a_0 = '1'$ to our string $w$, the lamp is plugged in.
* Using $\Delta$, we determine that from state $q_0$ with input of $1$ transition to the next state, $q_2$, which asks if the 'Bulb is bured out?'
* We check the bulb, and determine that no, the bulb is not burned out. This adds the character $a_1 = '0'$ to our string $w$.
* Again using $\Delta$, from state $q_2, with an input of $0$, we transition to our halting state, $q_4$, and determine that the lamp is indeed in need of serious repair.
* We built the string $w = '1', '0'$, given as input to our flow chart machine $M$, will always end in the halting state $q_4$ - the lamp needs serious repair.

So, our little machine shows us something interesting - for a given input string $w$, it will always yeild the same halting state. This establishes what is meant by 'deterministic' - given a particular input, into our machine, we'll always get the same result, or 'output'.

So, what exactly is a 'Deterministic Finite Automaton'? Automaton means machine, ie, a set of states with a transition table. Finite is a bit pedantic, but means that our input string has a finite number of characters, or more simply put 'the input string ends at some point'. And finally, our machine is deterministic - it always yields tho same output for a given input.

Well, as far as machines go, our flow chart machine $M$ isn't very impressive. Nevertheless, it serves as a decent example for how simple automata works. This is haldly, by any standards, what we would call a 'computer'. This will however, allow us to build up to more complex machines, and a more complete model for modern automata, to eventually gain a complete grasp on the theoretical model behind a true computer.

### Example: Regular expressions

A practical and widely used application of finite automata is the use of 'regular expressions'. A very common problem that many computer programs and applications must tackle is something called 'string manipulation'. String manipulation usually involves finding a particular pattern of characters in a human readable text string, and replacing it with something else. You're probably familiar with word processing problems, such as 'Google Docs' or 'Microsoft Word', and most likely you've used their 'find/replace' feature. This is a very simplistic example of string manipulation - the word processor will find the word (or so called 'substring') and replace it with another substring.

Regular expressions allow for extremely powerful manipulations of strings. For instance, suppose I wanted to see if a string contained the words 'her' or 'hair'. I could bulid the regular expression 'h(e|ai)r' ^[the braces and pipe character denote a logical 'or', saying h followed by ('e' OR 'ai') followed by r should be accepted], which would result in the following DFA:

![simple regular expression DFA](img/dfa.png) [@regexfsm]

The input strings $her$ and $hair$ will both end in the accepting halting state $4$ ^[by convention, accepting halting states are denoted by a double circle]. The string  $his$ though, will stop after $h$ has been entered, and it will not be accepted - it will transition to the invisible 'rejecting halting state'.

More complicated regular expressions are frequently used behind the scenes all around us. For instance, if you've ever signed up for an account on a website, you probably had to enter an email address. It's fairly likely that the service you signed up with used a regular expression to determine if your email address was indeed valid ^[though, incidentally, a truly correct regular expression for this particular problem is [notoriously difficult](http://www.regular-expressions.info/email.html)].

Regular expressions range from the very simple, to extremely complicated. Typically a regex is constructed as an NFA, which if you recall from early may be converted to a DFA. All major programming languages will offer some sort of regular expression library, with the de facto standard being [PCRE](http://www.pcre.org/), based off of the regular expression syntax used in the popular programming language 'perl'. Usually, regular expressions will be 'compiled' into their DFA form, which tends to be more simpler, and more efficient to evaluate.

Sufficed to say, regular expressions are quite a powerful tool and a great illustration of a practical application of finite automata. There are numerous resources available to tinker with regular expressions.

## Pushdown automata

To make a more interesting machine, we can add another element to our automata - a special type of state that is shared by the other discrete states - something called a stack. It's very likely that you're familiar with the idea of a stack of cards from playing any number of card games. The idea behind the stack is pretty simple - visualize a physical stack of cards. We can do to things - add a card to the stack, or remove it. Conventionally, we call adding something to a stack 'pushing' it onto the stack, and removing something from a stack 'popping' it from the stack. Stacks act in a 'first in, last out' manner - if I push a card onto the stack, then push a second card, I pop the second card first. This is why we call thess machines 'Pushdown' automata - the push cards down to the bottom of the stack, by piling new cards on top of them, and removing the new cards first.

Pushdown automata are similar to our finite state automata, but this stack enables them to do something pretty cool. When deciding which state to transition to, they don't just consider their current state and their input - they also consider the top card on the stack. When doing the transition, they can also push something else onto the stack, which will affect how the next state transition happens. In this sense, the stack is a sort of 'shared state' between all other states - we can use it to pass information along, for future state transitions.

If we revise our model state transition table from earlier, we need to add two more columns - one to consider the current element of the stack, and one to potentially push something onto the stack:

|Current state | Input |Top of Stack|  Next state | Push to stack |
|--------------|-------|------------|-------------|---------------|
|              |       |            |             |               |


## Turing machines



### Alan Turing - the tragic father of the modern age of computing

Turing machines are the namesake of the British mathematician, Alan Turing ^[A recently released movie, 'The Immitation Game', is a dramatic portrayal of his life and work]. Alan Turing was a mathematician who enjoyed problem solving, and was particularly interested in the field of Cryptography. Cryptography uses mathematical principles to take a obfuscate messages. The practical use of cryptography is in encrypting and decrypting messages for private and secure transmission.

Nowadays, we take cryptography for granted - for instance, when you browse the internet you've probably seen a green lock in our web browser - this means the data you are transmitting is being securely encrypted. Likewise, most cell phones have their contents encrypted so that if the phone is lost or stolen, the culprite won't be able to access our private data. Well before these applications however, cryptography had been used in military applications. The famous 'Caesar Cipher' was used by Julias Caesar to send coded messages to his generals. If these messages were intercepted by the enemy, they would would appear as nonsense, and wouldn't offer a tactical advantage. During Alan Turing's time, there was a particularly complicated cipher used during World War II by the Axis forces.

A special machine, called the [Enigma machine](https://en.wikipedia.org/wiki/Enigma_machine) was constructed to encrypt and decrypt messages, such that a the machine decrypted a message would need to be tuned in the same way as the machine that encrypted the message. The Axis forces would send an encrypted message each day, saying what the tuning for the next day should be. This meant that even if an Enigma machine were captured, it would only be useful for decoding the messages sent that day, and then be useless thereafter.


### Simulation

### Halting problem and infinite loops

# Digital logic, discrete mathematics

Numbers are made up. The ones we use happen to be based on the Arabic number system, which is happens to be base 10. This means that when we count, we 'roll over' the digits at 10. For this reason, there are only 10 symbols - 0 through 9. When we run out of those digits, we add another digit to the left - of increased 'order'. For instance, when we add 1 to 9, we get 10. Since we're out of digits, we add the digit '1' in a new, higher order, and roll back our lower order number to 0. Likewise, when we add 1 to 99, we roll over the lower two orders to '00' and add a new digit '1' of a higher order, making 100. You've probably noticed a pattern - each order is exactly 10 times larger than the previous order.

Long before we standardized on the base 10 system, there were actually other number systems! Some are still around. For instance, you are already intuitively aware of the base 60 number system - that is, if you can tell time! There are 60 seconds to a minute, 60 minutes to an hour. Once we have more than 60 seconds, we roll over and increment the number of minutes. Likewise, when 60 minutes have accumulated, we increase the number of hours.

There are two other number systems that we will need to understand in order to gain a better view into the internal workings of a computer - binary, and hexadecimal.

## Binary

We introduced binary in the last section, where we used it as the input language in our flow chart machine. This wasn't necessary, but it wasn't an accident either - this is the actual language computers understand and operate on. Exactly why computers used binary will be covered shortly, but sufficed to say, it's important to gain a grasp on what exactly it is.

The base 10 number system we are used to is so called because there are 10 digits to work with. In binary, there are only 2 digits to work with - 0, and 1. Thus, binary is considered a base 2 number system. Counting in binary is pretty easy - it's exactly the same as counting in base 10 ^[or at is more frequently called 'decimal' notation]. We start with 0, and add 1. Well, that's all we've got for digits! In order to add 1 again, we'll have to increase the order, so we go from '1' to '10'. To add one yet again, we get to '11'. Adding 1 again, we get '100'. Reading this in decimal looks quite weird - 0, 1, 10, 11, 100, 101, etc. The chart below counts from 0 to 16 in binary. The key thing to remember is that all of our number principles hold the same - when we run out of digits, we push a new number on the left side, which has a higher order. Each higher order number is exactly 2 times larger, instead of 10 times larger as with decimal.

| Decimal | Binary |
|---------|--------|
| 0       |  0     |
| 1       |  1     |
| 2       |  10    |
| 3       |  11    |
| 4       |  100   |
| 5       |  101   |
| 6       |  110   |
| 7       |  111   |
| 8       |  1000  |
| 9       |  1001  |
| 10      |  1010  |
| 11      |  1011  |
| 12      |  1100  |
| 13      |  1101  |
| 14      |  1110  |
| 15      |  1111  |
| 16      |  10000 |

If we look closely at this table, we can see a few patterns:

* Odd numbers end with a 1 - well, that's handy!
* Likewise, all even numbers end with 0.
* Look at any number, then look at the number that is double that number. For instance, 3 is '11' and 6 is '110', 7 is '111', and 14 '1110', 6 is '110', 12 is '1100'. To double any number, we just have to add 0 to the right side - neat!
* Any number directly preceeding a power of two will always be all 1's. Look at 3, 7, and 15.

As we add higher order numbers, we are increasing the number of 'bits'. For instance, we need only a single bit to encode 0 or 1, but we need 4 bits to encode numbers higher than 8, and less than 16. Likewise, we'll need 5 bits to go from 16 to 31, and so on. As an exercise, try counting all the way up to 32. All of these above properties should hold true.

So, how do we actually read this chicken scratch? Well, to decode binary into decimal, we have to do a bit of math unfortunately. Luckily, all we need to know is basic addition, and how to compute powers of two, both of which are easy enough. What we do is look at each digit, and multiply it by two to the power of its order, then add them together.

For instance, looking at the number 11. This is written in binary as 1011. This works out to be, from left to right, $1 * 2^3 + 0 * 2^2 + 1 * 2^1 + 1 * 2^0$ which we can simplify to $8 + 0 + 2 + 1$. Sure enough, adding that all togther, we get 11.

That's it for our crash course on binary! This will come in handy shortly, while we gain an understanding of boolean operations, digital logic, and binary arithmetic.

## Hexadecimal

Usually when we look at binary output, don't actually want to read it as a sequence of 0's and 1's. This is because, unforunately, our brains and eyes aren't quite as adept as a computer at consuming long streams of monotonous digits.

To make binary a bit easy to read, by convention we usually reperesent binary as something called 'hexadecimal'. Hexa meaning '6', and decimal meaning '10', hexadecimal is an extension of our familiar decimal system, but instead of being base 10, it's base 16. Unfortunately, we only have 10 symbols at our disposal! So, we grudgingly have to add 6 more, and being rather uninventive, we just pull them from our normal alphabet. Thus, hexadecimal is the digits 0-9, including letters A through F, the first 6 letters of the alphabet. By convention, hex is usually written in capital letters, but doesn't need to be.

One of the reasons why hexedecimal is so useful is because it being a base 16 number system, it can encode 4 digits worth of binary into just one character. That's pretty handy, as it cuts the size down by a factor of 4. Computers also typically deal with binary numbers in a particular set 'word' size. That is, if you have a 16 bit computer processor, the word size of the system will be 16 bits. Nowadays, you've probably heard marketing jargon about '64' bit and '32' bit processors. This is the maximum size of numbers that the computer can work with in hardware.

By convention, we usually do something called 'zero padding' the left size of a binary number to a particular word size. For instance, if our word size is 16, we start with 16 0's, instead of just a single 0. Revisiting our table from earlier, we'll see how the decimal corresponds to zero-padded binary, and hexadecimal:

| Decimal | Binary                | Hexadecimal |
|---------|-----------------------|-------------|
| 0       |  0000 0000 0000 0000  | 0000        |
| 1       |  0000 0000 0000 0001  | 0001        |
| 2       |  0000 0000 0000 0010  | 0002        |
| 3       |  0000 0000 0000 0011  | 0003        |
| 4       |  0000 0000 0000 0100  | 0004        |
| 5       |  0000 0000 0000 0101  | 0005        |
| 6       |  0000 0000 0000 0110  | 0006        |
| 7       |  0000 0000 0000 0111  | 0007        |
| 8       |  0000 0000 0000 1000  | 0008        |
| 9       |  0000 0000 0000 1001  | 0009        |
| 10      |  0000 0000 0000 1010  | 000A        |
| 11      |  0000 0000 0000 1011  | 000B        |
| 12      |  0000 0000 0000 1100  | 000C        |
| 13      |  0000 0000 0000 1101  | 000D        |
| 14      |  0000 0000 0000 1110  | 000E        |
| 15      |  0000 0000 0000 1111  | 000F        |
| 16      |  0000 0000 0001 0000  | 0010        |

Well, that's not very exciting is it! But notice a couple of things:

* We group the zero-padded binary into groups of 4 bits, each hexedecimal character represents one of these.
* All of the hexadecimal numbers from 0-9 are the same, and after that we just keep on counting with letters.
* F is the new 9, so to speak, once we've hit F, we need to increase our order to get any higher.
* The hexadecimal '10' is actually '16 in decimal'... that might get confusing, since they use the same digits!

We don't get a whole lot of value out of writting out the padded zeros in hexadecimal, so we can often abbreviate this by ommitting the zeros altogether. To tell the difference between decimal and hexadecimal, we often preceed hexadecimal with '0x'. So for instance if we ready 0x10, we know that is 16 in hexadecimal, **not** 10 in decimal.

Hexadecimal is going to come in real handy later on, when we closely examine the internal operations of a computer processor. The main thing you need to know about hexadecimal is that it's really just a convenient shorthand. You might be asking 'why bother with hexadecimal at all; why not just use decimal?' The reasoning there is convenience - 16 is a power of 2, 10 is not. As mentioned earlier, since 16 is $2^4$, we can encode 4 bits of binary in a single hexadecimal character. While we're abbreviating and coming up with shorthands, let's also shorten 'hexadecimal' to simply 'hex'^[this is not to be confused with the kind if hex that a witch or wizard might use, rather, it's just computer scientists being lazy].

## Boolean logic

* AND, OR, NOT
* Transistors / D-latches

# Computer internals and architecture

How does a computer actually work

A great reference is Computer Organization and Design [@cod] (textbook)


## CPU and instruction sets

* Note about special registers and operations, how context switching works

* Instruction pipeline
* ALU
* Memory pyramid

## Practical computing


Van Neuman Machine

Harvard architecture

Little man computer

CISC, RISC



# Networking



## Fundamentals


Packets and frames
Mac addresses, IP addresses, ARP




## Layer 2 networks and broadcast domains


Switching and hubs
ARP storms and stale ARP
Gratuitous ARP


## Layer 3 networks


Routing and routing protocols
CIDR notation
Gateways
Route tables
BGP, RIP, OSPF
Autonomous systems


## Layer 4 transport protocols


TCP vs UDP
TCP sessions and overhead
SYN/ ACK handshakes
Data gram anatomy
Socket bindings
Passing data in buffers and kernel space


Layer 5 - session
Layer 6 - encryption
Layer 7 - application


Useful protocols to know
HTTP
DHCP
SNMP
Conceptual FTP (similarities to HTTP)
Note about SSL

# Operating System Theory

# Kernel

Contexts, resources, hardware, threads, user space, kernel space

Pages and virtual memory

# Linux

Start with disclaimer about other operating systems, largely similar but opaque and proprietary. Linux being open source makes it great to study, and it's ubiquity makes it highly practical to understand.

Choose Gentoo as a case study due to quality of documentation

# Linux kernel

Building it
Customizing config options
Overview of different types of options
Notable features
Netfilter, IPtables
Cgroups and namespaces (footnote about containers)
Driver support, file system formats
Scheduling algorithms
Modules vs builtin
Kprobes and Uprobes (footnote about systemtap)
Kexec


Loading the operating system



# Boot methods


PXE
Steps, including DHCP, TFTP, potentially HTTP
Tangent about iPXE
Undi format


Disk
LILO
Syslinux / ISOLINUX (footnote on PXELINUX)
Grub


Vmlinuz and initramfs


## Kernel booting

Decompressing itself
Loading initial objects into memory
Discovering devices
Potentially running initramfs
Setting up the root volume


# Linux file system structure



Etc
Boot
Usr
Lib
Var
Tmp
Mnt
Run


Pseudo and special file systems
Tmpfs
Dev
Proc
Sys


Files and folders, inodes, symlinks, hardlinks



Union file systems



Squashfs, overlayfs



# Init

Different Init systems, history, inittab, runlevels

Runit, systemd, upstart, openrc

Systemd

Daemons

Popular daemons like Apache, nginx, ssh, MySQL, Redis, Memcache


# Realtime systems

# System administration

## Testing infrastructure and systems

## Configuration management

## Disposable and immutable infrastructure


# Containers

## Fundamentals

### chroot

### cgroup

### namespaces

## lxc

## docker



# Programming languages

Start with C

Static, dynamic, interpreted, native

Compiler linker, runtime linker,  ELF binaries'


byte code interpreter, JIT compilers, FLEX

Turing completeness

Specific ones ( C, C++, Java, Python, ruby, go)

Garbage collection

Reference counting

# Data structures and algorithms

Queue, stack, graph, tree, hash table

Hash functions

Search

Np hard


# Concurrent and parallel programming

Concurrent clusters (sun grid engine, openmp, etc)

Frameworks

# Markup languages

# Web programming

# Data centres

What are they

Ping, power, pipe
Colo retail vs wholesale


Power distribution


At the data centre level - substations, gensets, power distribution units and wips
Diesel fuel


# Server hardware

* Sensors
* Backplanes
* Baseboards
* Raid cards
* Riser cards
* Nodes vs chassis
* Blades
* Components (drives, NICs)
* IPMI
* DMI, smbios


BIOS and EFI

# Network hardware

## Cable media types

* Types of optics
* Types of copper
* Twinax, molex
* CAB, etc

## Internals of a switch

router (fabrics, linecards)

# Cloud



# Embedded systems


Raspberry pi, micro controllers


# Application Architecture

# Databases and state


ORM
Models
Caching

# Encryption and security principles

## Theory

Cryptography
Entropy
Number spaces and languages


## Ciphers

Block ciphers - AES
RSA


# Artificial intelligence and machine learning


Neural networks
Regression
Statistics
Genetic algorithms


# Data mining and warehousing


Kafka
Hadoop
Map reduce
Columnar data stores
