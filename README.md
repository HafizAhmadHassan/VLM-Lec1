## CNN Problems

- Long range dependency problem e.g someone illegally crossing the road

- the reacher who awas explaining difficult concept to students smiled
  
- Thanks to attention mechanism to solve.

## Attention Mechanism

- Journey of Token
- we will be tracking journey of token
- it has famous architecture
- we will understand architecture Encoder e.g BERT and Decoder e.g GPT (8:40)


### LLM

- input --> LLM Model ->  next word prediction (10:52)

- whats happen inside LLM?
  - Alternate Representation of Decoder (12:00)
  - we split it in 3 parts
    1. input
    2. transformer block Multihead attention block
    3. output block

## Lets see input Block

- input text
- tokenised text
- token embedding
- positional embedding (13:29)

##  Input Sentence
1. Split into tokens 14:19
2. simplicity token is one word
3. Lets see "cat" what happen to this token 
4. there are different ways to split characters or subwords

## Spliting Basis 15:42
- Word based :
  - Numbers of token not that high if character more tokens
  - OOV problem out of vocab words "ozempic" not part of dictionary we nned to handle
  - cat, cats another token
  - taking care of root words eat + ing
  - 170K to 200K words in English

- Character based
  - 26 english vocab 
  - no issue OOV
  - meanings between the words entirely lost
  - Tokenised sequence much bigger

- Byte-Pair Encoding (22:40)
  1. 4 pairs of cc
  2. 2 pairs of ca

- we will choose cc to encode
- for each pair there is token id for example cc contains 

## Exoloration of Cat (25:47)
- GPT total tokens 50257
- dictionary help convert words to token id
 
## Token Embeddings 27:00
- 768 dimention GPT-2
- the word mat --<id> --> represent the word in 768 dimension to represent features about the token
- for example token is noun, is gender , is verb , is emotion etc ?
- every vector[0] element tell somethings about word
- Check the graph (29:15)
- for more. 

- cat is represented 768 dimensional space

## Use Case 30:30 

- the dog is chasing another dog
- i cannot represent two dogs with same feature vector.
- because one dog is chasing 
- another is being chased
- so position of words also carrying the information

## Another case 31:20
- all dogs are animals
- all animals are dogs
- so position matters in this case
- Positional embeddings are also in 768 dimension
- we have vectors contains bits on/off or float value
- 32:18 
- [is this token at begging? , is this token at middlem ....., is there long-range dependency, isemotion?]
- methamitical representation is difficult to say we just for the moment say it as abraction point of view
- cat --> positional embeddings 768 + 768 Token embddings

## Visual 33:42
- when positional embedding are added
- this is how u encode 
- more visual (35:00)
- Result : input embeddings

## Zoom Out Visual 36:00
- Tokenized text BPE
- token Embedding : pick vector from dictionary 
- Positional : element wise addition 

## Visual (37:30)
- Step one give 768 dimension pass to transformer Block

## Visual transformer (38:40)
1. 2 layers
2. 2 drop out
3. 2 addition
4. Feedforward NN
5. Multihead attention

## Transformer Block (40:00)

- input token goes here 768
- 2 skip connections

## Layer normalisation (41:00)
- Batch Normalisation 
- 800 images contains 8 images in batches we have 100 batches
- Layer we apply to every token
- calculate 768 dimension mean and standard deviation
- xi -mean / std
- after apply this formula to vector mean of new vector become zero 
- std deviation
- over normalisation is not good
- if numbers are coming from wide varitey of ranges it is not good thing
  - results: vanishing gradient
  - exploding gradient
- Another problem is constraining the distribution of data.
  - How we solve ?
    - multiply Bi plus gemma(i)
    - beta is scaling and gemma is bias
    - this is trainable parameter
    - Reason mean and std varies according to 
    - prevent over standardization
  
## Beta and Gemma (47:24)
- beta and gemma are learnable parameters
- [[xi- mean]/std]* beta-i + gemma(i)
- what if beta and gemma are having values 1 and 0 respectively. then it will not have any effect
- for example in data distribution of 768 dimension vector v
- v[50] is extreme value which we need to retain
- beta and gemma will help us to retain that extreme value and prevent over stadandisation.
- in actual case beta and Gemma has different values
- if you did not do this one shot?

## Layer normalisation when to apply?
- would be understood by hyperparameter tuning-ì.
- realised through hit and trial
- this is happen every single layer normalisation

## 3 Layer Normalisation 48:53
- in gpt we have 3.

## DropOut reducing Overfitting Mechanism 51:07 
- regularisation
- to prevent overfitting
- Lazy neurons vs active neuron
- Lazy means if i switch off it ... it does not affect output
- I switched off active neurons heavy lifting .. the remaining neurons will participate in output
- forcing lazy neurons to participate in reducing loss
- red are masked
- we dont manually check . we can give value 0.2 fraction to neurons dropout.
- 80 % participate if scale down neurons output also scale down to 80%
- if output scale down by 2/3
- then we have scale up 3/2 neurons as well the remaining one later on
- look (54:20 ) 
   
## Scaling factor ( 54:20 )
- how scaling reduced by 20%
- lets say w[1,1,1,1] and x[0.5,0.5,0.5,0.5]
- output= sum(w*x) = [2] if i mask 2 neurons then w[1,0,0,1] so output will be 2
- how can i can make sure output same i scale up by 50% which is 1/0.5

## Resnet Skip Connection  (57:00)
- gradient of last layer influence to ouput more than first layer
- its called vanishing gradient
- w1 new = w1 old - n* gradient(Loss/w1) --- less impact because less change
- we want all layers participate?
- add previous layers alongside with next layer so output have an affect and participation  (1:00:09)
- look at plot it wont vanish any more

### thats why there are skip connections in transformer block we do after normalisation (1:01:)
- we fount its more good
- entire architecture (1:02) there are multiple skip connection
- earlier layers have also impact to architecture

## GPT-3 
- 96 transformer block 
- 96 multihead attention in parallel
- in every singlr transformer block there are 96 heads
- numbers depends on GPT versions

## Attention Learning phase
1. Simplified self attention
2. self attention
3. causual attention
4. multihead attention
- GPT- 3 12288 - dimention token /96 - 96 multihead attention 
- if 768 dimension coming each head taking care of 64 
- there latent multi head attention different types of attention mechanism
- we will focues on casual attention
- we do it later (1:08:00)

## FeedForward (1:10)
- we are looking journey of token
- 768 is dimension of cat 
- projected up 768x 4 
- and projected down to 768
- 768 arsinel to represent meaning of cat
- why we dont make down and make up (1:12:00)
- because we lost information 

## How it look like mathematical ops?
- 1x768 
- 768x4 
- 1x3072
- why 4 you try out different params and check which one fit it
- bring back 1x768
- what u use for vector transformation
- matrix multiplication (1:15)
- They all are learnable parameters
- GPT has 175B params roughlt 2/3 parmas are feedforward
- rest of params in multihead (1:18)
- it distributed accross  

## Each token Journet to Pass? (1:19)
- pass to 12 transformer
- input --> 768 and output --> 768
- they are connected linearly 


## Lets zoom out (1:20)
- look image
- dropout overfitting
- layer normal
- feedforward

## Lets zoom out whole (1:21)
- review it

## Lets see Output Part
- Final layer normalisation
- (xi - mean /std ) * B + Gemma
- cat expereinced lot of things GPT-2 pass through 12 and GPT-3 96 blocks
- when u come output layer

## What i want in output? (1:26)
- i want next word prediction
- i am going to one word 
- 50000 words in my dictionary
- 768 dimension 
- i got 50257 words
- 1x 768 x 768 x 50257
- results =1x50257 --> pass to softmax
- result vector i will see probablity distribution 1x 50257 --> these are called logits... their values could be negative floats--> which we pass through softmax --> result vector will be in range
- For example there are multiple chances of choosing words with small probablity difference (1:30)
- there is guarantee highest probablity should be picked 

## Final output (1:33)
- it shows training and prediction

## Zoom out Output Step (1:34)
- review it

## Question?
- The cat sat on the mat
- cat will have information of all the tokens or just previous?
- we will se in casual attention later on
- We will see RNN attention idea

## Next Lecture
- RNN + Bahdanau + Simplifield self attention
- self attention + casual + multihead
- LLM--> ViT --> code from Scratch
