# Solidity contract audition bot powered by ChatGPT

## Table of Contents
* [Background](#Background)
* [Possible usage and shortage of current ChatGPT demo](#Possible-usage-and-shortage-of-current-ChatGPT-demo)
* [Install](#Install)
* [Features](#Features)
* [Examples](#Examples)
* [Contributing](#Contributing)
* [License](#License)

----------
### Background

AI is definitely a hit since OpenAI released ChatGPT at the end of November 2022. Basically, every fintech company claimed to integrate AI into their operational logic and VCs are extremely FOMO on spilling money on unicorns. Even some traditional companies which have been listed on the US stock market pumped their stock price by including AI in service.   

As a degen in DeFi and a newbie in AI, I do want to learn this promissing tech by combining this two areas. Obviously, I am not qualified to create an algorithm and train my own model to compete with OpenAI. Instead, I can build some interesting bot based on a universal one -- ChatGPT.

---

### Possible usage and shortage of current ChatGPT demo

#### Usage

The use case is as important as the technology itself. 
I believe most ppl don't understand what digital 3d and IMAX can do before *Avatar I* showed on screen.
AI is the same situation. ChatGPT is a huge usage that excited ppl worldwide, but I expect much more than that.

To list some possible usage:
1. Software for Office  
Notion and DeepL has already made use of AI to enhance user experience.   
Since Office is one of core products of Microsoft, AI will be included sooner or later.   
There is no doubt that some imitators in China like WPS will quickly follow up.   
It's safe to say AI is going to be a default mechanism in all kinds of software for the office.

2. Content Creator  
ChatGPT has shown that AI can do much better and faster on summary or expand contents around some topics.   
Advertisment, work summary even grantuate eassay can be finished by ChatGPT without major flaws.   
NFTs are indispenable on blockchain and the creation threshold has been significantly lowered with Midjourney.   
Considering ChatGPT is good at describing, while Midjourney is good at drawing, it's possible to combine these two to deliver a more user-friendly product.

3. AI Lawyer  
An US enterprise called DoNotPay has been using AI to help ppl defend themselves in some minor caces like traffic violation.  
Recently, the CEO of it offered 1 M reward to anyone who can use AI lawyer on larger case in Supreme Court.  
Although this area might stand on the edge of law, but still make sense.  
Think about it, lawyers took massively thick documents into court just to choose previous trials in customers' favor.  
And it's impossible to remember all of them, but AI can fetch relevant data with some fine-tune process.  
If both plaintiff and defendant are armed with AI, the trial proceedings should be fairer.  

4. Boosted Search Engine  
To be honest, ChatGPT has been a partner for me when coding, including this project.   
In some specific areas, ChatGPT is extremely powerful compared with Google and Bing.   
Programming is one of them. Microsoft planned to integrate ChatGPT into Bing in March and some source said Baidu would release its own chat bot in the same month.   
Not to mention Google, I sincerely think Google has been improving its own AI for search engine, which is definitely not inferior to OpenAI.   
This area is massive and many vulnerabilities could cause bugs, but it's always good to try.  

5. Education Assistant (for students and teacher)  
AI may be unwelcom to many education institution.   
The main concern is on cheating. The thing is initial purpose of education is passing knowledge instead of grading students.   
Thus, if AI can truly make education more efficient and fairer, then it's nonsense to reject this tech.  

What AI can actually help with education? These points are from ChatGPT:  
* Automated grading and feedback
* Personalized learning and adaptive tutoring
* Automated course planning and scheduling
* Automated student guidance and counseling
* Automated assessment and evaluation
* Adaptive learning options
* Virtual assistants for students
* Monitoring student progress and engagement

#### Shortage

Currently, the shortage of ChatGPT is pretty clear as well.
1. When it comes to task covers less well-trodden ground, ChatGPT will probably deliever wrong answer with confident tone.   
   Take the example of Vitalik. He asked ChatGPT to write a PLONK verifier, and ChatGPT gave a hilarious answer. The right answer should be like [this](https://github.com/ethereum/research/blob/master/py_plonk/verifier.py).

![ChatGPT-reply](https://vitalik.ca/images/gpt3/plonk.png 'ChatGPT-reply')

2. ChatGPT got goldfish memory.  
   The public latest text model of OpenAI is text-davinci-03, which has a limitation of 4000 tokens (roughly 160000 characters).
   Thus, it can only remember a conversation with 160000 characters, and that is far from enough when it comes to complex work like contract audition.
   Creating a bot with long-term memory is essential for complex tasks.

3. Mulitple retries are needed.
   As far as I know, ChatGPT needs retry to get answer for two reasons.
    - High request demands   
      Users around the world give prompt, waiting for reply. ChatGPT is overworked in some specific situation.
    - Inaccurate answer     
      Sometimes, it takes multiple retries to get the ideal answer, even if the prompt is the same.

-----
### Install

1. Download slither analyzer to scan vulnerabilities in solidity files
```
$ pip3 install slither-analyzer
```

2. Download openai to get completion of prompt
```
$ pip install openai
```

3. Get audition bot from github
```
$ git clone https://github.com/jasonthewhale/GPT_AUDIT.git
```

-----
### Features

This bot simulates the process of auditing contracts. It finishes these tasks by order:  
  1. Scan all vulnerabilities in all solidity files provided, and this relies on slither;
  2. Create embedded data for contract contents, and it will check similarity and use relevant data as prompt; (text-similarity-ada-001 model)
  3. Give a summary of how these contracts work with each other based on files and one centence description from user;
  4. Store all chat logs, and choose X most relevant logs as part of prompt;
  5. Answer all kinds of detailed questions (any variable or function) based on ChatGPT's knowledge. (text-davinci-003 model)


------
### Examples
1. Put all the solidity files into "solidity_folder". Slither and ChatGPT will scan and audit these files.
   Example:  
 ```
   solidity_file    
      - ERC20.sol    
      - ERC721.sol   
      - ERC1155.sol  
 ```         

                
2. Run this line:  

```
$ python3 -i ama.py  
```
  - First, it will merge all contract contents into a single file, called "merge.txt":   
    expected output:    
    ```
    $ finished merge contract
    ```
  - then it will create embedding data with contract contents, saving as "index.json":  
    expected output:    
    ```  
    $ contract embedding has created
    ```
  - Second, it will run slither to scan all vulnerabilities, and print results in terminal:  
    ```
    $ Context._msgData() (solidity_folder/utils/Context.sol#21-23) is never used and should be removed
    $ ERC20._burn(address,uint256) (solidity_folder/ERC20.sol#277-293) is never used and should be removed
    $ ERC20._mint(address,uint256) (solidity_folder/ERC20.sol#251-264) is never used and should be removed
    $ Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#dead-code

    $ Pragma version^0.8.0 (solidity_folder/ERC20.sol#4) allows old versions
    $ Pragma version^0.8.0 (solidity_folder/IERC20.sol#4) allows old versions
    $ Pragma version^0.8.0 (solidity_folder/extensions/IERC20Metadata.sol#4) allows old versions
    $ Pragma version^0.8.0 (solidity_folder/utils/Context.sol#4) allows old versions
    $ solc-0.8.13 is not recommended for deployment
    $ Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#incorrect-versions-of-solidity
    $ ./solidity_folder/ERC20.sol analyzed (4 contracts with 81 detectors), 8 result(s) found
    ```
  - Third, it will get all the function names from "merge.txt" and ask user to give a brief description of this project.
    the pre-prompt would look like:   
    > Assume you are a solidity contract audit bot, called AVI. USER will give you all names of contracts in his project and a brief description of his         project. Please tell user your thoughts of how these functions work together.
    >
    > Contract names: PotionPair, PotionPairETH, PotionPairERC20, PotionPool, PotionFactory, PotionRouter  
    > Description: my project's name is Potion Protocol, aiming to provide an AMM trding platform for NFTs  
  - With these useful information, the audit chat bot is ready to answer all relevant questions without limitation to tokens.
    ```
    AVI: Based on the contract names and project description, my understanding is that PotionPair is a contract used to pair NFTs with their corresponding     ERC20 and ETH tokens, PotionPool is a contract used to manage a liquidity pool of NFT and token pairs, PotionFactory is a contract used to mint and       burn NFTs, and PotionRouter is a contract used to route token transfers to and from the liquidity pool. The contracts should work together to enable       the user to trade NFTs on the AMM platform.
    USER: 
    ```
    
   - Also, bot will automatically store all chat log which can be used to get previous conversation, making more accurate answer.
    
---
### Contributing    

---

### License
MIT © Junchen You

