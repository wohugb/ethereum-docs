# 以太坊：一个安全分散的广义交易分类帐

拜占庭版本56c4578  -  2018-02-02

博士,加文木创始人, 以太坊和平价 gavin@parity.io

## 抽象

区块链与加密担保交易相结合，已经通过一些项目证明了它的实用性，比特币是最值得关注的项目之一。
每个这样的项目都可以看作是一个分散的单一计算资源的简单应用。,我们可以将这个范例称为具有共享状态的事务性单例机器。

以太坊以一种广义的方式实现这个范例。
此外，它提供了多个这样的资源，每个资源具有不同的状态和操作码，但是能够通过消息传递框架与其他人交互。
我们讨论其设计，实施问题，提供的机会以及我们设想的未来障碍。

## 1. 介绍

With ubiquitous internet connections in most places of the world, global information transmission has become incredibly cheap. Technology-rooted movements like Bit- coin have demonstrated through the power of the default, consensus mechanisms, and voluntary respect of the social contract, that it is possible to use the internet to make a decentralised value-transfer system that can be shared across the world and virtually free to use. This system can be said to be a very specialised version of a cryptograph- ically secure, transaction-based state machine. Follow-up systems such as Namecoin adapted this original “currency application” of the technology into other applications al- beit rather simplistic ones.
Ethereum is a project which attempts to build the gen- eralised technology; technology on which all transaction- based state machine concepts may be built. Moreover it aims to provide to the end-developer a tightly integrated end-to-end system for building software on a hitherto un- explored compute paradigm in the mainstream: a trustful object messaging compute framework.

### 1.1. Driving Factors

There are many goals of this project; one key goal is to facilitate transactions be- tween consenting individuals who would otherwise have no means to trust one another. This may be due to geographical separation, interfacing difficulty, or perhaps the incompatibility, incompetence, unwillingness, expense, uncertainty, inconvenience or corruption of existing legal systems. By specifying a state-change system through a rich and unambiguous language, and furthermore archi- tecting a system such that we can reasonably expect that an agreement will be thus enforced autonomously, we can provide a means to this end.
Dealings in this proposed system would have several attributes not often found in the real world. The incor- ruptibility of judgement, often difficult to find, comes nat- urally from a disinterested algorithmic interpreter. Trans- parency, or being able to see exactly how a state or judge- ment came about through the transaction log and rules or instructional codes, never happens perfectly in human- based systems since natural language is necessarily vague,
information is often lacking, and plain old prejudices are difficult to shake.
Overall, I wish to provide a system such that users can be guaranteed that no matter with which other individ- uals, systems or organisations they interact, they can do so with absolute confidence in the possible outcomes and how those outcomes might come about.

### 1.2. Previous Work

Buterin [2013a] first proposed the kernel of this work in late November, 2013. Though now evolved in many ways, the key functionality of a block- chain with a Turing-complete language and an effectively unlimited inter-transaction storage capability remains un- changed.
Dwork and Naor [1992] provided the first work into the usage of a cryptographic proof of computational expendi- ture (“proof-of-work”) as a means of transmitting a value signal over the Internet. The value-signal was utilised here as a spam deterrence mechanism rather than any kind of currency, but critically demonstrated the potential for a basic data channel to carry a strong economic signal, allowing a receiver to make a physical assertion without having to rely upon trust. Back [2002] later produced a system in a similar vein.
The first example of utilising the proof-of-work as a strong economic signal to secure a currency was by Vish- numurthy et al. [2003]. In this instance, the token was used to keep peer-to-peer file trading in check, ensuring “consumers” be able to make micro-payments to “suppli- ers” for their services. The security model afforded by the proof-of-work was augmented with digital signatures and a ledger in order to ensure that the historical record couldn’t be corrupted and that malicious actors could not spoof payment or unjustly complain about service deliv- ery. Five years later, Nakamoto [2008] introduced an- other such proof-of-work-secured value token, somewhat wider in scope. The fruits of this project, Bitcoin, became the first widely adopted global decentralised transaction ledger.
Other projects built on Bitcoin’s success; the alt-coins introduced numerous other currencies through alteration to the protocol. Some of the best known are Litecoin and Primecoin, discussed by Sprankel [2013]. Other projects sought to take the core value content mechanism of the
1

 A.
Where Ω is the block-finalisation state transition func- tion (a function that rewards a nominated party); B is this block, which includes a series of transactions amongst some other components; and Π is the block-level state- transition function.
This is the basis of the blockchain paradigm, a model that forms the backbone of not only Ethereum, but all de- centralised consensus-based transaction systems to date.

### 2.1. Value

In order to incentivise computation within the network, there needs to be an agreed method for trans- mitting value. To address this issue, Ethereum has an in- trinsic currency, Ether, known also as ETH and sometimes referred to by the Old English  ̄D. The smallest subdenom- ination of Ether, and thus the one in which all integer val- ues of the currency are counted, is the Wei. One Ether is defined as being 1018 Wei. There exist other subdenomi- nations of Ether:
Multiplier Name
100 Wei 1012 Szabo 1015 Finney 1018 Ether
Throughout the present work, any reference to value, in the context of Ether, currency, a balance or a payment, should be assumed to be counted in Wei.

### 2.2. Which History

Since the system is decentralised and all parties have an opportunity to create a new block on some older pre-existing block, the resultant structure is necessarily a tree of blocks. In order to form a consensus as to which path, from root (the genesis block) to leaf (the block containing the most recent transactions) through this tree structure, known as the blockchain, there must be an agreed-upon scheme. If there is ever a disagree- ment between nodes as to which root-to-leaf path down the block tree is the ‘best’ blockchain, then a fork occurs.
This would mean that past a given point in time (block), multiple states of the system may coexist: some nodes believing one block to contain the canonical transac- tions, other nodes believing some other block to be canoni- cal, potentially containing radically different or incompat- ible transactions. This is to be avoided at all costs as the uncertainty that would ensue would likely kill all confi- dence in the entire system.
ETHEREUM: A SECURE DECENTRALISED GENERALISED TRANSACTION LEDGER BYZANTIUM VERSION 56c4578 - 2018-02-02 2
protocol and repurpose it; Aron [2012] discusses, for ex- ample, the Namecoin project which aims to provide a de- centralised name-resolution system.
Other projects still aim to build upon the Bitcoin net- work itself, leveraging the large amount of value placed in the system and the vast amount of computation that goes into the consensus mechanism. The Mastercoin project, first proposed by Willett [2013], aims to build a richer protocol involving many additional high-level features on top of the Bitcoin protocol through utilisation of a num- ber of auxiliary parts to the core protocol. The Coloured Coins project, proposed by Rosenfeld [2012], takes a sim- ilar but more simplified strategy, embellishing the rules of a transaction in order to break the fungibility of Bit- coin’s base currency and allow the creation and tracking of tokens through a special “chroma-wallet”-protocol-aware piece of software.
Additional work has been done in the area with discard- ing the decentralisation foundation; Ripple, discussed by Boutellier and Heinzen [2014], has sought to create a “fed- erated” system for currency exchange, effectively creating a new financial clearing system. It has demonstrated that high efficiency gains can be made if the decentralisation premise is discarded.
Early work on smart contracts has been done by Szabo [1997] and Miller [1997]. Around the 1990s it became clear that algorithmic enforcement of agreements could become a significant force in human cooperation. Though no spe- cific system was proposed to implement such a system, it was proposed that the future of law would be heavily affected by such systems. In this light, Ethereum may be seen as a general implementation of such a crypto-law system.
For a list of terms used in this paper, refer to Appendix

## 2. The Blockchain Paradigm

reference. Blocks function as a journal, recording a series of transactions together with the previous block and an identifier for the final state (though do not store the final state itself—that would be far too big). They also punc- tuate the transaction series with incentives for nodes to mine. This incentivisation takes place as a state-transition function, adding value to a nominated account.
Mining is the process of dedicating effort (working) to bolster one series of transactions (a block) over any other potential competitor block. It is achieved thanks to a cryptographically secure proof. This scheme is known as a proof-of-work and is discussed in detail in section 11.5.
Formally, we expand to:
(2) σt+1 ≡ (3)B≡ (4) Π(σ,B) ≡
Π(σt,B)
(..., (T0 , T1 , ...))
Ω(B, Υ(Υ(σ, T0), T1)...)
  Ethereum, taken as a whole, can be viewed as a transaction-based state machine: we begin with a gene- sis state and incrementally execute transactions to morph it into some final state. It is this final state which we ac- cept as the canonical “version” of the world of Ethereum. The state can include such information as account bal- ances, reputations, trust arrangements, data pertaining to information of the physical world; in short, anything that can currently be represented by a computer is admis- sible. Transactions thus represent a valid arc between two states; the ‘valid’ part is important—there exist far more invalid state changes than valid state changes. Invalid state changes might, e.g., be things such as reducing an account balance without an equal and opposite increase elsewhere. A valid state transition is one which comes about through a transaction. Formally:
(1) σt+1 ≡Υ(σt,T)
where Υ is the Ethereum state transition function. In Ethereum, Υ, together with σ are considerably more pow- erful then any existing comparable system; Υ allows com- ponents to carry out arbitrary computation, while σ al- lows components to store arbitrary state between trans- actions.
Transactions are collated into blocks; blocks are chained together using a cryptographic hash as a means of

 ETHEREUM: A SECURE DECENTRALISED GENERALISED TRANSACTION LEDGER BYZANTIUM VERSION 56c4578 - 2018-02-02 3
The scheme we use in order to generate consensus is a simplified version of the GHOST protocol introduced by Sompolinsky and Zohar [2013]. This process is described in detail in section 10.
Sometimes, a path follows a new protocol from a partic- ular height. This document describes one version of the protocol. In order to follow back the history of a path, one might need to reference multiple versions of this doc- ument.

## 3. Conventions

I use a number of typographical conventions for the formal notation, some of which are quite particular to the present work:
The two sets of highly structured, ‘top-level’, state val- ues, are denoted with bold lowercase Greek letters. They fall into those of world-state, which are denoted σ (or a variant thereupon) and those of machine-state, μ.
Functions operating on highly structured values are denoted with an upper-case Greek letter, e.g. Υ, the Ethereum state transition function.
For most functions, an uppercase letter is used, e.g. C, the general cost function. These may be subscripted to denote specialised variants, e.g. CSSTORE, the cost func- tion for the SSTORE operation. For specialised and possibly externally defined functions, I may format as typewriter text, e.g. the Keccak-256 hash function (as per the winning entry to the SHA-3 contest) is denoted KEC (and generally referred to as plain Keccak). Also KEC512 is referring to the Keccak 512 hash function.
Tuples are typically denoted with an upper-case letter, e.g. T, is used to denote an Ethereum transaction. This symbol may, if accordingly defined, be subscripted to re- fer to an individual component, e.g. Tn, denotes the nonce of said transaction. The form of the subscript is used to denote its type; e.g. uppercase subscripts refer to tuples with subscriptable components.
Scalars and fixed-size byte sequences (or, synony- mously, arrays) are denoted with a normal lower-case let- ter, e.g. n is used in the document to denote a transaction nonce. Those with a particularly special meaning may be Greek, e.g. δ, the number of items required on the stack for a given operation.
Arbitrary-length sequences are typically denoted as a bold lower-case letter, e.g. o is used to denote the byte sequence given as the output data of a message call. For particularly important values, a bold uppercase letter may be used.
Throughout, we assume scalars are positive integers and thus belong to the set P. The set of all byte sequences is B, formally defined in Appendix B. If such a set of se- quences is restricted to those of a particular length, it is denoted with a subscript, thus the set of all byte sequences of length 32 is named B32 and the set of all positive in- tegers smaller than 2256 is named P256. This is formally defined in section 4.3.
Square brackets are used to index into and reference individual components or subsequences of sequences, e.g. μs[0] denotes the first item on the machine’s stack. For subsequences, ellipses are used to specify the intended range, to include elements at both limits, e.g. μm[0..31] denotes the first 32 items of the machine’s memory.
In the case of the global state σ, which is a sequence of accounts, themselves tuples, the square brackets are used to reference an individual account.
When considering variants of existing values, I follow the rule that within a given scope for definition, if we assume that the unmodified ‘input’ value be denoted by the placeholder   then the modified and utilisable value is denoted as  ′, and intermediate values would be  ∗,  ∗∗ &c. On very particular occasions, in order to max- imise readability and only if unambiguous in meaning, I may use alpha-numeric subscripts to denote intermediate values, especially those of particular note.
When considering the use of existing functions, given a function f, the function f∗ denotes a similar, element- wise version of the function mapping instead between se- quences. It is formally defined in section 4.3.
I define a number of useful functions throughout. One of the more common is l, which evaluates to the last item in the given sequence:
(5)
l(x) ≡ x[∥x∥ − 1]

## 4. Blocks, State and Transactions

Having introduced the basic concepts behind Ethereum, we will discuss the meaning of a transaction, a block and the state in more detail.

### 4.1. World State.

The world state (state), is a map- ping between addresses (160-bit identifiers) and account states (a data structure serialised as RLP, see Appendix B). Though not stored on the blockchain, it is assumed that the implementation will maintain this mapping in a modified Merkle Patricia tree (trie, see Appendix D). The trie requires a simple database backend that maintains a mapping of bytearrays to bytearrays; we name this under- lying database the state database. This has a number of benefits; firstly the root node of this structure is crypto- graphically dependent on all internal data and as such its hash can be used as a secure identity for the entire sys- tem state. Secondly, being an immutable data structure, it allows any previous state (whose root hash is known) to be recalled by simply altering the root hash accordingly. Since we store all such root hashes in the blockchain, we are able to trivially revert to old states.
The account state comprises the following four fields:
nonce: A scalar value equal to the number of trans- actions sent from this address or, in the case of accounts with associated code, the number of contract-creations made by this account. For ac- count of address a in state σ, this would be for- mally denoted σ[a]n.
balance: A scalar value equal to the number of Wei owned by this address. Formally denoted σ[a]b.
storageRoot: A 256-bit hash of the root node of a Merkle Patricia tree that encodes the storage con- tents of the account (a mapping between 256-bit integer values), encoded into the trie as a map- ping from the Keccak 256-bit hash of the 256-bit integer keys to the RLP-encoded 256-bit integer values. The hash is formally denoted σ[a]s.
codeHash: The hash of the EVM code of this account—this is the code that gets executed should this address receive a message call; it is

 (7)
(8)
where:
LI  (k, v)  ≡  KEC(k), RLP(v)  k ∈ B32 ∧ v ∈ P
ETHEREUM: A SECURE DECENTRALISED GENERALISED TRANSACTION LEDGER BYZANTIUM VERSION 56c4578 - 2018-02-02 4
immutable and thus, unlike all other fields, can- not be changed after construction. All such code fragments are contained in the state database un- der their corresponding hashes for later retrieval. This hash is formally denoted σ[a]c, and thus the code may be denoted as b, given that KEC(b) = σ[a]c.
Since I typically wish to refer not to the trie’s root hash
but to the underlying set of key/value pairs stored within,
I define a convenient equivalence:
 ∗
(6) TRIE LI (σ[a]s) ≡ σ[a]s
The collapse function for the set of key/value pairs in the trie, L∗I , is defined as the element-wise transformation of the base function LI , given as:
nonce: A scalar value equal to the number of trans- actions sent by the sender; formally Tn.
gasPrice: A scalar value equal to the number of Wei to be paid per unit of gas for all computa- tion costs incurred as a result of the execution of this transaction; formally Tp.
gasLimit: A scalar value equal to the maximum amount of gas that should be used in executing this transaction. This is paid up-front, before any computation is done and may not be increased later; formally Tg.
to: The 160-bit address of the message call’s recipi- ent or, for a contract creation transaction, ∅, used here to denote the only member of B0 ; formally Tt.
value: A scalar value equal to the number of Wei to be transferred to the message call’s recipient or, in the case of contract creation, as an endowment to the newly created account; formally Tv.
v, r, s: Values corresponding to the signature of the transaction and used to determine the sender of the transaction; formally Tw, Tr and Ts. This is expanded in Appendix F.
Additionally, a contract creation transaction contains:
init: An unlimited size byte array specifying the EVM-code for the account initialisation proce- dure, formally Ti.
init is an EVM-code fragment; it returns the body, a second fragment of code that executes each time the account receives a message call (either through a trans- action or due to the internal execution of code). init is executed only once at account creation and gets discarded immediately thereafter.
In contrast, a message call transaction contains: data: An unlimited size byte array specifying the
input data of the message call, formally Td.
Appendix F specifies the function, S, which maps transactions to the sender, and happens through the ECDSA of the SECP-256k1 curve, using the hash of the transaction (excepting the latter three signature fields) as the datum to sign. For the present we simply assert that the sender of a given transaction T can be represented with S(T).
It shall be understood that σ[a]s is not a ‘physical’ member of the account and does not contribute to its later serialisation.
If the codeHash field is the Keccak-256 hash of the empty string, i.e. σ[a]c = KEC () , then the node repre- sents a simple account, sometimes referred to as a “non- contract” account.
Thus we may define a world-state collapse function LS : LS(σ) ≡ {p(a) : σ[a] ̸= ∅}
p(a) ≡  KEC(a), RLP (σ[a]n, σ[a]b, σ[a]s, σ[a]c)
(9) where (10)
This function, LS, is used alongside the trie function to provide a short identity (hash) of the world state. We assume:
(11) ∀a:σ[a]=∅ ∨ (a∈B20 ∧ v(σ[a]))
where v is the account validity function:
(12) v(x) ≡ xn ∈ P256∧xb ∈ P256∧xs ∈ B32∧xc ∈ B32
An account is empty when it has no code, zero nonce and zero balance:
(13)
EMPTY(σ,a) ≡ σ[a]c =KEC () ∧σ[a]n =0∧σ[a]b =0
Even callable precompiled contracts can have an empty account state. This is because their account states do not usually contain the code describing its behavior.
An account is dead when its account state is non- existent or empty:
(14) DEAD(σ, a) ≡ σ[a] = ∅ ∨ EMPTY(σ, a)
4.2. The Transaction. A transaction (formally, T) is a single cryptographically-signed instruction constructed by an actor externally to the scope of Ethereum. While it is assumed that the ultimate external actor will be human in nature, software tools will be used in its construction and dissemination1. There are two types of transactions: those which result in message calls and those which result in the creation of new accounts with associated code (known informally as ‘contract creation’). Both types specify a number of common fields:
(15)
LT (T ) ≡
 (Tn,Tp,Tg,Tt,Tv,Ti,Tw,Tr,Ts) (Tn, Tp, Tg, Tt, Tv, Td, Tw, Tr, Ts)
if Tt = ∅ otherwise
Here, we assume all components are interpreted by the RLP as integer values, with the exception of the arbitrary length byte arrays Ti and Td.
(16)
where
(17)
Tv ∈P256 Td∈B∧Ti∈B
Tn ∈P256 ∧ Tg∈P256 ∧ Ts∈P256 ∧
Pn ={P :P ∈P∧P <2n}
∧ Tp ∈P256 ∧ Tw∈P5 ∧ Tr∈P256 ∧
 1Notably, such ‘tools’ could ultimately become so causally removed from their human-based initiation—or humans may become so causally-neutral—that there could be a point at which they rightly be considered autonomous agents. e.g. contracts may offer bounties to humans for being sent transactions to initiate their execution.

 ETHEREUM: A SECURE DECENTRALISED GENERALISED TRANSACTION LEDGER BYZANTIUM VERSION 56c4578 - 2018-02-02 5
The address hash Tt is slightly different: it is either a 20-byte address hash or, in the case of being a contract- creation transaction (and thus formally equal to ∅), it is the RLP empty byte sequence and thus the member of B0:
nonce: A 64-bit hash which, combined with the mix-hash, proves that a sufficient amount of com- putation has been carried out on this block; for- mally Hn.
The other two components in the block are simply a list of ommer block headers (of the same format as above) and a series of the transactions. Formally, we can refer to a block B:
(19) B ≡ (BH,BT,BU)
4.3.1. Transaction Receipt. In order to encode informa- tion about a transaction concerning which it may be use- ful to form a zero-knowledge proof, or index and search, we encode a receipt of each transaction containing cer- tain information from concerning its execution. Each re- ceipt, denoted BR[i] for the ith transaction, is placed in an index-keyed trie and the root recorded in the header as He.
The transaction receipt is a tuple of four items com- prising the cumulative gas used in the block containing the transaction receipt as of immediately after the trans- action has happened, Ru, the set of logs created through execution of the transaction, Rl and the Bloom filter com- posed from information in those logs, Rb and the status code of the transaction, Rz:
(20) R ≡ (Ru, Rb, Rl, Rz)
The function LR trivially prepares a transaction receipt
for being transformed into an RLP-serialised byte array: (21) LR(R) ≡ (0 ∈ B256, Ru, Rb, Rl)
where 0 ∈ B256 replaces the pre-transaction state root that existed in previous versions of the protocol.
We assert that the status code Rz is an integer. (22) Rz ∈ P
We assert Ru, the cumulative gas used is a positive in- teger and that the logs Bloom, Rb, is a hash of size 2048 bits (256 bytes):
(23) Ru ∈P ∧ Rb ∈B256
The sequence Rl is a series of log entries, (O0,O1,...). A log entry, O, is a tuple of the logger’s address, Oa, a series of 32-byte log topics, Ot and some number of bytes of data, Od:
(24) O ≡ (Oa, (Ot0, Ot1, ...), Od)
(25) Oa ∈B20 ∧ ∀t∈Ot :t∈B32 ∧ Od ∈B
We define the Bloom filter function, M, to reduce a log entry into a single 256-byte hash:
(18) Tt ∈
 B20 ifTt̸=∅ B0 otherwise
4.3. The Block. The block in Ethereum is the collec- tion of relevant pieces of information (known as the block header), H, together with information corresponding to the comprised transactions, T, and a set of other block headers U that are known to have a parent equal to the present block’s parent’s parent (such blocks are known as ommers2). The block header contains several pieces of information:
parentHash: The Keccak 256-bit hash of the par- ent block’s header, in its entirety; formally Hp.
ommersHash: The Keccak 256-bit hash of the om- mers list portion of this block; formally Ho.
beneficiary: The 160-bit address to which all fees collected from the successful mining of this block be transferred; formally Hc.
stateRoot: The Keccak 256-bit hash of the root node of the state trie, after all transactions are executed and finalisations applied; formally Hr.
transactionsRoot: TheKeccak256-bithashofthe root node of the trie structure populated with each transaction in the transactions list portion of the block; formally Ht.
receiptsRoot: The Keccak 256-bit hash of the root node of the trie structure populated with the re- ceipts of each transaction in the transactions list portion of the block; formally He.
logsBloom: The Bloom filter composed from in- dexable information (logger address and log top- ics) contained in each log entry from the receipt of each transaction in the transactions list; formally Hb.
difficulty: A scalar value corresponding to the dif- ficulty level of this block. This can be calculated from the previous block’s difficulty level and the timestamp; formally Hd.
number: A scalar value equal to the number of an- cestor blocks. The genesis block has a number of zero; formally Hi.
gasLimit: A scalar value equal to the current limit of gas expenditure per block; formally Hl.
gasUsed: A scalar value equal to the total gas used in transactions in this block; formally Hg.
timestamp: A scalar value equal to the reasonable output of Unix’s time() at this block’s inception; formally Hs.
extraData: An arbitrary byte array containing data relevant to this block. This must be 32 bytes or fewer; formally Hx.
mixHash: A256-bithashwhich,combinedwiththe nonce, proves that a sufficient amount of compu- tation has been carried out on this block; formally Hm.
(26) M(O) ≡
   M3:2048(t)  t∈{Oa }∪Ot
where M3:2048 is a specialised Bloom filter that sets three bits out of 2048, given an arbitrary byte sequence. It does this through taking the low-order 11 bits of each
 2ommer is the most prevalent (not saying much) gender-neutral term to mean “sibling of parent”; see https://nonbinary.miraheze.org/ wiki/Gender_neutral_language#Aunt.2FUncle
311 bits = 22048, and the low-order 11 bits is the modulo 2048 of the operand, which is in this case is “each of the first three pairs of bytes in a Keccak-256 hash of the byte sequence.”

 ETHEREUM: A SECURE DECENTRALISED GENERALISED TRANSACTION LEDGER BYZANTIUM VERSION 56c4578 - 2018-02-02 6
of the first three pairs of bytes in a Keccak-256 hash of the byte sequence.3 Formally:
We now have a rigorous specification for the construc- tion of a formal block structure. The RLP function RLP (see Appendix B) provides the canonical method for trans- forming this structure into a sequence of bytes ready for transmission over the wire or storage locally.
4.3.4. Block Header Validity. We define P(BH) to be the parent block of B, formally:
(39) P (H) ≡ B′ : KEC(RLP(BH′ )) = Hp
The block number is the parent’s block number incre-
mented by one:
(40) Hi ≡P(H)Hi +1
The canonical difficulty of a block of header H is de- fined as D(H):
(41)
(27)M3:2048(x : x ∈ B) ≡
y : y ∈ B256 (0, 0, ..., 0)
where: except:
(28) (29) (30)
y = ∀i∈{0,2,4} :
m(x, i) ≡
Bm(x,i)(y) = 1
KEC(x)[i, i + 1] mod 2048
where B is the bit reference function such that Bj(x) equals the bit of index j (indexed from 0) in the byte array x.
4.3.2. Holistic Validity. We can assert a block’s validity if and only if it satisfies several conditions: it must be in- ternally consistent with the ommer and transaction block hashes and the given transactions BT (as specified in sec 11), when executed in order on the base state σ (derived from the final state of the parent block), result in a new state of the identity Hr:
 D0
max D0,P(H)Hd +x×ς2 +ε
D0 ≡ 131072  P(H)Hd
(31)
D(H)≡ where:
if Hi=0 otherwise

,−99
Hr ≡
Ho ≡
Ht ≡
He ≡
Hb ≡
∧
TRIE(LS (Π(σ, B)))
KEC(RLP(L∗H (BU )))
TRIE({∀i < ∥BT∥, i ∈ P : p(i, LT (BT[i]))}) TRIE({∀i < ∥BR∥, i ∈ P : p(i, LR(BR[i]))})
 rb
where p(k, v) is simply the pairwise RLP transformation,
the block and the second being the transaction receipt: (32) p(k, v) ≡  RLP(k), RLP(v)
∧ (42) ∧
∧
(43) (44)
x≡ 2048
 r∈BR
in this case, the first being the index of the transaction in

ς2≡max x−
 Hs −P(H)Hs  9
 Furthermore:
(33) TRIE(LS (σ)) = P (BH )H r (45)
x≡
ε ≡  2⌊Hi′÷100000⌋−2
Thus TRIE(LS(σ)) is the root node hash of the Merkle Patricia tree structure containing the key-value pairs of the state σ with values encoded using RLP, and P(BH) is the parent block of B, defined directly.
The values stemming from the computation of transac- tions, specifically the transaction receipts, BR, and that defined through the transactions state-accumulation func- tion, Π, are formalised later in section 11.4.

### 4.3.3. Serialisation.

The function LB and LH are the preparation functions for a block and block header respec- tively. Much like the transaction receipt preparation func- tion LR, we assert the types and order of the structure for when the RLP transformation is required:
(34) LH(H) ≡ ( Hp,Ho,Hc,Hr,Ht,He,Hb,Hd, Hi,Hl,Hg,Hs,Hx,Hm,Hn )
(35) LB(B) ≡  LH(BH),L∗ (BT),L∗ (BU)  TH
With L∗T and L∗H being element-wise sequence trans- formations, thus:
(36)
(46)
Hi′ ≡ max(Hi − 3000000, 0)
f∗ (x0, x1, ...)  ≡  f(x0), f(x1), ...
The component types are defined thus:
Note that D0 is the difficulty of the genesis block. The Homestead difficulty parameter, ς2, is used to affect a dy- namic homeostasis of time between blocks, as the time be- tween blocks varies, as discussed below, as implemented in EIP-2 by Buterin [2015]. In the Homestead release, the exponential difficulty symbol, ε causes the difficulty to slowly increase (every 100,000 blocks) at an exponen- tial rate, and thus increasing the block time difference, and putting time pressure on transitioning to proof-of- stake. This effect, known as the “difficulty bomb”, or “ice age”, was explained in EIP-649 by Schoedon and Buterin [2017] and delayed and implemented earlier in EIP-2. ς2 was also modified in EIP-100 with the use of x, the ad- justment factor above, and the demoninator 9, in order to target the mean block time including uncle blocks by Buterin [2016]. Finally, in the Byzantium release, with EIP-649, the ice age was delayed by creating a fake block number, Hi′, which is obtained by substracting three mil- lion from the actual block number, which in other words reduced ε and the time difference between blocks, in order to allow more time to develop proof-of-stake and prevent- ing the network from “freezing” up.
The canonical gas limit Hl of a block of header H must fulfil the relation:
(37) Hp ∈ B32 Hr ∈ B32
Hb ∈ B256 Hl ∈ P Hx ∈ B
∧ Ho∈B32 ∧ Ht∈B32 ∧ Hd∈P
∧ Hg∈P ∧Hm∈B32
for any function f ∧ Hc∈B20 ∧
∧ He∈B32 ∧ ∧ Hi∈P∧ ∧ Hs∈P256 ∧ ∧ Hn∈B8
(47) Hl (48) Hl
(49) Hl
 P(H)Hl   <P(H)Hl+ 1024 ∧
 P(H)Hl   >P(H)Hl− 1024 ∧
  5000
 1 if∥P(H)U∥=0 2 otherwise
 where
(38) Bn ={B:B∈B∧∥B∥=n}

 ETHEREUM: A SECURE DECENTRALISED GENERALISED TRANSACTION LEDGER BYZANTIUM VERSION 56c4578 - 2018-02-02 7
Hs is the timestamp (in Unix’s time()) of block H and must fulfil the relation:
(50) Hs >P(H)Hs
This mechanism enforces a homeostasis in terms of the time between blocks; a smaller period between the last two blocks results in an increase in the difficulty level and thus additional computation required, lengthening the likely next period. Conversely, if the period is too large, the difficulty, and expected time to the next block, is reduced.
The nonce, Hn, must satisfy the relations:
2256
Hd
with (n, m) = PoW(Hn, Hn, d).
Where Hn is the new block’s header H, but without the nonce and mix-hash components, d being the current DAG, a large data set needed to compute the mix-hash, and PoW is the proof-of-work function (see section 11.5): this evaluates to an array with the first item being the mix- hash, to proof that a correct DAG has been used, and the second item being a pseudo-random number cryptograph- ically dependent on H and d. Given an approximately uniform distribution in the range [0,264), the expected time to find a solution is proportional to the difficulty, Hd.
This is the foundation of the security of the blockchain and is the fundamental reason why a malicious node can- not propagate newly created blocks that would otherwise overwrite (“rewrite”) history. Because the nonce must satisfy this requirement, and because its satisfaction de- pends on the contents of the block and in turn its com- posed transactions, creating new, valid, blocks is difficult and, over time, requires approximately the total compute power of the trustworthy portion of the mining peers.
Thus we are able to define the block header validity function V (H):
(52) V(H) ≡ (53)
(54) (55)
(56)
(57) (58) (59) (60)
making message calls, utilising and accessing account stor- age and executing operations on the virtual machine) has a universally agreed cost in terms of gas.
Every transaction has a specific amount of gas associ- ated with it: gasLimit. This is the amount of gas which is implicitly purchased from the sender’s account balance. The purchase happens at the according gasPrice, also specified in the transaction. The transaction is considered invalid if the account balance cannot support such a pur- chase. It is named gasLimit since any unused gas at the end of the transaction is refunded (at the same rate of pur- chase) to the sender’s account. Gas does not exist outside of the execution of a transaction. Thus for accounts with trusted code associated, a relatively high gas limit may be set and left alone.
In general, Ether used to purchase gas that is not re- funded is delivered to the beneficiary address, the address of an account typically under the control of the miner. Transactors are free to specify any gasPrice that they wish, however miners are free to ignore transactions as they choose. A higher gas price on a transaction will there- fore cost the sender more in terms of Ether and deliver a greater value to the miner and thus will more likely be selected for inclusion by more miners. Miners, in general, will choose to advertise the minimum gas price for which they will execute transactions and transactors will be free to canvas these prices in determining what gas price to offer. Since there will be a (weighted) distribution of min- imum acceptable gas prices, transactors will necessarily have a trade-off to make between lowering the gas price and maximising the chance that their transaction will be mined in a timely manner.

### 6. Transaction Execution

The execution of a transaction is the most complex part of the Ethereum protocol: it defines the state transition function Υ. It is assumed that any transactions executed first pass the initial tests of intrinsic validity. These in- clude:
(1) The transaction is well-formed RLP, with no ad- ditional trailing bytes;
(2) the transaction signature is valid;
(3) the transaction nonce is valid (equivalent to the
sender account’s current nonce);
(4) the gas limit is no smaller than the intrinsic gas,
g0, used by the transaction;
(5) the sender account balance contains at least the
cost, v0, required in up-front payment.
Formally, we consider the function Υ, with T being a
transaction and σ the state:
(61) σ′=Υ(σ,T)
Thus σ′ is the post-transactional state. We also define Υg to evaluate to the amount of gas used in the execu- tion of a transaction, Υl to evaluate to the transaction’s accrued log items and Υz to evaluate to the status code resulting from the transaction. These will be formally de- fined later.

### 6.1. Substate.

Throughout transaction execution, we accrue certain information that is acted upon immediately
(51) n
∧ m=Hm
   2256 Hd
∧ m = Hm ∧ Hg≤Hl ∧
 P(H)Hl   Hl<P(H)Hl+ 1024 ∧
 P(H)Hl   Hl>P(H)Hl− 1024 ∧
Hl 5000∧ Hs>P(H)Hs ∧ Hi=P(H)Hi+1 ∧ ∥Hx∥ ≤ 32
n   Hd=D(H)∧
   where (n, m) = PoW(Hn, Hn, d)
Noting additionally that extraData must be at most
32 bytes.

## 5. Gas and Payment

In order to avoid issues of network abuse and to side- step the inevitable questions stemming from Turing com- pleteness, all programmable computation in Ethereum is subject to fees. The fee schedule is specified in units of gas (see Appendix G for the fees associated with var- ious computation). Thus any given fragment of pro- grammable computation (this includes creating contracts,

 ETHEREUM: A SECURE DECENTRALISED GENERALISED TRANSACTION LEDGER BYZANTIUM VERSION 56c4578 - 2018-02-02 8
following the transaction. We call this transaction sub- state, and represent it as A, which is a tuple:
(62) A ≡ (As, Al, At, Ar)
The tuple contents include As, the self-destruct set: a set of accounts that will be discarded following the trans- action’s completion. Al is the log series: this is a series of archived and indexable ‘checkpoints’ in VM code exe- cution that allow for contract-calls to be easily tracked by onlookers external to the Ethereum world (such as decen- tralised application front-ends). At is the set of touched accounts, of which the empty ones are deleted at the end of a transaction. Finally there is Ar, the refund balance, increased through using the SSTORE instruction in or- der to reset contract storage to zero from some non-zero value. Though not immediately refunded, it is allowed to partially offset the total execution costs.
We define the empty substate A0 to have no self- destructs, no logs, no touched accounts and a zero refund balance:
(63) A0 ≡(∅,(),∅,0)

### 6.2. Execution.

We define intrinsic gas g0, the amount of gas this transaction requires to be paid prior to execu- tion, as follows:
We define the checkpoint state σ0:
(64) g0≡  i∈Ti ,Td
(65)
(66)
 Gtxdatazero Gtxdatanonzero
if i = 0 otherwise
 Gtxcreate 0
if Tt = ∅ otherwise
+
+ Gtransaction
where
action’s associated data and initialisation EVM-code, depending on whether the transaction is for contract- creation or message-call. Gtxcreate is added if the transac- tion is contract-creating, but not if a result of EVM-code. G is fully defined in Appendix G.
The up-front cost v0 is calculated as: (67) v0 ≡TgTp +Tv
The validity is determined as:
Ti , Td means the
series of bytes
of the
trans-
(69) σ0 ≡ (70) σ0[S(T)]b ≡ (71) σ0[S(T)]n ≡
σ except: σ[S(T)]b −TgTp σ[S(T)]n+1
Evaluating σP from σ0 depends on the transaction type; either contract creation or message call; we define the tuple of post-execution provisional state σP , remain- ing gas g′, substate A and status code z:
(72)
 Λ 4 ( σ 0 , S ( T ) , T o ,

where g is the amount of gas remaining after deducting the basic amount required to pay for the existence of the transaction:
(73) g≡Tg−g0
and To is the original transactor, which can differ from the sender in the case of a message call or contract creation not directly triggered by a transaction but coming from the execution of EVM-code.
Note we use Θ4 and Λ4 to denote the fact that only the first four components of the functions’ values are taken; the final represents the message-call’s output value (a byte array) and is unused in the context of transaction evalua- tion.
After the message call or contract creation is processed, the state is finalised by determining the amount to be re- funded, g∗ from the remaining gas, g′, plus some allowance from the refund counter, to the sender at the original rate.
(74) g∗≡g′+min{ Tg−g′ ,Ar} 2
The total refundable amount is the legitimately remain- ing gas g′, added to Ar, with the latter component being capped up to a maximum of half (rounded down) of the total amount used Tg − g′.
The Ether for the gas is given to the miner, whose ad- dress is specified as the beneficiary of the present block B. So we define the pre-final state σ∗ in terms of the provisional state σP :
(σP,g′,A,z)≡
if Tt = ∅  Tt, Tt, g, Tp, Tv, Tv, Td, 0, ⊤) otherwise
g,Tp,Tv,Ti,0,⊤) Θ4(σ0,S(T),To,
 (68)
S(T) ̸= σ[S(T)] ̸= Tn = g0   v0   Tg
∅ ∧
∅∧ σ[S(T)]n ∧ Tg ∧ σ[S(T)]b ∧ BHl−l(BR)u
Note the final condition; the sum of the transaction’s gas limit, Tg, and the gas utilised in this block prior, given by l(BR)u, must be no greater than the block’s gasLimit, BHl.
The execution of a valid transaction begins with an irrevocable change made to the state: the nonce of the ac- count of the sender, S(T), is incremented by one and the balance is reduced by part of the up-front cost, Tg Tp . The gas available for the proceeding computation, g, is defined as Tg − g0. The computation, whether contract creation or a message call, results in an eventual state (which may legally be equivalent to the current state), the change to which is deterministic and never invalid: there can be no invalid transactions from this point.
The final state, σ′, is reached after deleting all accounts that either appear in the self-destruct list or are touched and empty:
(75) (76) (77) (78)
σ∗ ≡ σP except σ∗[S(T)]b ≡ σP[S(T)]b +g∗Tp
σ∗[m]b ≡ σP [m]b + (Tg − g∗)Tp m ≡ BHc
(79) (80) (81)
σ′ ≡ σ∗ except ∀i∈As :σ′[i] = ∅
∀i ∈ At : σ′[i] = ∅ if DEAD(σ∗,i)
And finally, we specify Υg, the total gas used in this transaction, Υl, the logs created by this transaction and Υz, the status code of this transaction:
(82) Υg(σ,T) (83) Υl(σ,T) (84) Υz(σ,T)
≡Tg−g′ ≡ Al ≡z

 ETHEREUM: A SECURE DECENTRALISED GENERALISED TRANSACTION LEDGER BYZANTIUM VERSION 56c4578 - 2018-02-02 9
These are used to help define the transaction receipt, discussed later.

### 7. Contract Creation

There are a number of intrinsic parameters used when creating an account: sender (s), original transactor (o), available gas (g), gas price (p), endowment (v) together with an arbitrary length byte array, i, the initialisation EVM code and finally the present depth of the message- call/contract-creation stack (e).
We define the creation function formally as the func- tion Λ, which evaluates from these values, together with the state σ to the tuple containing the new state, remain- ing gas, accrued transaction substate and an error message (σ′, g′, A, o), as in section 6:
(85) (σ′,g′,A,z,o) ≡ Λ(σ,s,o,g,p,v,i,e,w)
The address of the new account is defined as being the rightmost 160 bits of the Keccak hash of the RLP encod- ing of the structure containing only the sender and the nonce. Thus we define the resultant address for the new account a:

(86) a ≡ B96..255 KEC RLP (s, σ[s]n − 1)
where KEC is the Keccak 256-bit hash function, RLP is the RLP encoding function, Ba..b(X) evaluates to binary value containing the bits of indices in the range [a,b] of the binary data X and σ[x] is the address state of x or ∅ if none exists. Note we use one fewer than the sender’s nonce value; we assert that we have incremented the sender ac- count’s nonce prior to this call, and so the value used is the sender’s nonce at the beginning of the responsible transaction or VM operation.
The account’s nonce is initially defined as one, the bal- ance as the value passed, the storage as empty and the code hash as the Keccak 256-bit hash of the empty string; the sender’s balance is also reduced by the value passed. Thus the mutated state becomes σ∗:
ment as defined in section 9, that is:
(89) (90)
σ∗[a] =
σ∗[s] =
σ∗≡σ except:
 1, v + v′, TRIE(∅), KEC ()
 ∅ ifσ[s]=∅ ∧ v=0 a∗ otherwise
a∗ ≡
where v′ is the account’s pre-existing value, in the event
Id evaluates to the empty tuple as there is no input data to this call. IH has no special treatment and is de- termined from the blockchain.
Code execution depletes gas, and gas may not go below zero, thus execution may exit before the code has come to a natural halting state. In this (and several other) exceptional cases we say an out-of-gas (OOG) exception has occurred: The evaluated state is defined as being the empty set, ∅, and the entire create operation should have no effect on the state, effectively leaving it as it was im- mediately prior to attempting the creation.
If the initialization code completes successfully, a fi- nal contract-creation cost is paid, the code-deposit cost, c, proportional to the size of the created contract’s code:
(102) c ≡ Gcodedeposit × |o|
If there is not enough gas remaining to pay this, i.e. g∗∗ < c, then we also declare an out-of-gas exception.
The gas remaining will be zero in any such exceptional condition, i.e. if the creation was conducted as the recep- tion of a transaction, then this doesn’t affect payment of the intrinsic cost of contract creation; it is paid regardless. However, the value of the transaction is not transferred to the aborted contract’s address when we are out-of-gas.
If such an exception does not occur, then the remain- ing gas is refunded to the originator and the now-altered state is allowed to persist. Thus formally, we may spec- ify the resultant state, gas, substate and status code as (σ′, g′, A, z) where:
(87) (88)
(91) v′≡ 0 σ[a]b
(σ[s]n,σ[s]b −v,σ[s]s,σ[s]c) it was previously in existence:
(103)
g′ ≡ (104)
σ′ ≡ (105)
z ≡
where (106)
F ≡
 0 ifF

if σ[a]=∅ otherwise
g∗∗ − c
σ 
 σ∗∗
otherwise
Finally, the account is initialised through the execution of the initialising EVM code i according to the execution model (see section 9). Code execution can effect several events that are not internal to the execution state: the account’s storage can be altered, further accounts can be created and further message calls can be made. As such, the code execution function Ξ evaluates to a tuple of the resultant state σ∗∗, available gas remaining g∗∗, the ac- crued substate A and the body code of the account o.
(92) (σ∗∗, g∗∗, A, o) ≡ Ξ(σ∗, g, I, {s, a})
where I contains the parameters of the execution environ-
except:  σ′[a] = ∅
if F
if DEAD(σ∗∗, a) otherwise
(93)
(94)
(95)
(96)
(97)
(98)
(99)
(100) Ie (101) Iw
≡ a ≡ o ≡ p ≡ () ≡ s ≡ v ≡ i ≡ e ≡ w
 ∗∗ 
σ  
except: σ′[a]c = KEC(o)
Ia Io Ip Id Is Iv Ib
 0 if σ∗∗ =∅∨g∗∗ <c 1 otherwise
 (σ∗∗ =∅ ∧ o=∅)∨ g∗∗ <c ∨ |o|>24576

 ETHEREUM: A SECURE DECENTRALISED GENERALISED TRANSACTION LEDGER BYZANTIUM VERSION 56c4578 - 2018-02-0210
The exception in the determination of σ′ dictates that o, the resultant byte sequence from the execution of the initialisation code, specifies the final body code for the newly-created account.
Note that intention is that the result is either a suc- cessfully created new contract with its endowment, or no new contract with no transfer of value.
7.1. Subtleties. Note that while the initialisation code is executing, the newly created address exists but with no intrinsic body code. Thus any message call received by it during this time causes no code to be executed. If the initialisation execution ends with a SELFDESTRUCT instruction, the matter is moot since the account will be deleted before the transaction is completed. For a normal STOP code, or if the code returned is otherwise empty, then the state is left with a zombie account, and any re- maining balance will be locked into the account forever.

### 8. Message Call

In the case of executing a message call, several param-
eters are required: sender (s), transaction originator (o), recipient (r), the account whose code is to be executed (c, usually the same as recipient), available gas (g), value (v) and gas price (p) together with an arbitrary length byte array, d, the input data of the call and finally the present depth of the message-call/contract-creation stack (e).
Aside from evaluating to a new state and transaction substate, message calls also have an extra component—the output data denoted by the byte array o. This is ignored when executing transactions, however message calls can be initiated due to VM-code execution and in this case this information is used.
(107) (σ′,g′,A,z,o) ≡ Θ(σ,s,o,r,c,g,p,v,v ̃,d,e,w)
Note that we need to differentiate between the value that is to be transferred, v, from the value apparent in the ex- ecution context, v ̃, for the DELEGATECALL instruction.
We define σ1, the first transitional state as the orig- inal state but with the value transferred from sender to recipient:
(108) σ1[r]b ≡ σ[r]b + v ∧ σ1[s]b ≡ σ[s]b − v
unless s = r.
Throughout the present work, it is assumed that if
σ1[r] was originally undefined, it will be created as an ac- count with no code or state and zero balance and nonce.
The account’s associated code (identified as the frag- ment whose Keccak hash is σ[c]c) is executed according to the execution model (see section 9). Just as with contract creation, if the execution halts in an exceptional fashion (i.e. due to an exhausted gas supply, stack underflow, in- valid jump destination or invalid instruction), then no gas is refunded to the caller and the state is reverted to the point immediately prior to balance transfer (i.e. σ).
(115) σ′ ≡ (116) g′ ≡ z≡

ΞRIP160(σ1, g, I, t) 
Ξ (σ ,g,I,t) ID 1
ΞEXPMOD(σ1, g, I, t) 
(117()σ∗∗, g∗∗, A, o) ≡
if r=1 if r=2 if r=3 if r=4 if r=5 if r=6 if r=7 if r=8
Ξ(σ1, g, I, t) otherwise r
o
p
d
s
v ̃
e
w
{s, r}
σ[c]c
 σ ifσ∗∗=∅ σ∗∗ otherwise
 0 ifσ∗∗=∅∧o=∅ g∗∗ otherwise
 0 if σ∗∗=∅ 1 otherwise
ΞECREC(σ1, g, I, t) 
Ξ (σ ,g,I,t)  SHA256 1
ΞBN   ADD(σ1, g, I, t) 

Thus (109)
(110)
(111)
(112) (113)
the previous equation should be taken to mean: σ1 ≡ σ′1 except:
 ′
σ1[s]≡ ∅ ifσ1[s]=∅ ∧ v=0
a1 otherwise
a1 ≡ (σ′1[s]n, σ′1[s]b − v, σ′1[s]s, σ′1[s]c)
and σ′1≡σ except:
It is assumed that the client will have stored the pair (KEC(Ib ), Ib ) at some point prior in order to make the de- termination of Ib feasible.
As can be seen, there are eight exceptions to the usage of the general execution framework Ξ for evaluation of the message call: these are eight so-called ‘precompiled’ contracts, meant as a preliminary piece of architecture that may later become native extensions. The eight con- tracts in addresses 1 to 8 execute the elliptic curve public key recovery function, the SHA2 256-bit hash scheme, the RIPEMD 160-bit hash scheme, the identity function, arbi- trary precision modular exponentiation, elliptic curve ad- dition, elliptic curve scalar multiplication and an elliptic curve pairing check respectively.
σ′ [r] ≡ (0,v,TRIE(∅),KEC(())) 1
σ′1[r]≡∅ σ′1[r] ≡ a′1
if σ[r] = ∅ ∧ v ̸= 0 if σ[r]=∅∧v=0 otherwise
(114) a′1 ≡ (σ[r]n, σ[r]b + v, σ[r]s, σ[r]c)
Their full formal definition is in Appendix E.
Let

ΞBN   MUL(σ1, g, I, t) 


ΞSNARKV(σ1, g, I, t)
(118) (119) (120) (121) (122) (123) (124) (125) (126) (127)
Ia ≡ Io ≡ Ip ≡ Id ≡ Is ≡ Iv ≡ Ie ≡ Iw ≡
t ≡ KEC(Ib ) =

 ETHEREUM: A SECURE DECENTRALISED GENERALISED TRANSACTION LEDGER BYZANTIUM VERSION 56c4578 - 2018-02-0211

## 9. Execution Model

The execution model specifies how the system state is altered given a series of bytecode instructions and a small tuple of environmental data. This is specified through a formal model of a virtual state machine, known as the Ethereum Virtual Machine (EVM). It is a quasi-Turing- complete machine; the quasi qualification comes from the fact that the computation is intrinsically bounded through a parameter, gas, which limits the total amount of com- putation done.

### 9.1. Basics.

The EVM is a simple stack-based architec- ture. The word size of the machine (and thus size of stack item) is 256-bit. This was chosen to facilitate the Keccak- 256 hash scheme and elliptic-curve computations. The memory model is a simple word-addressed byte array. The stack has a maximum size of 1024. The machine also has an independent storage model; this is similar in concept to the memory but rather than a byte array, it is a word- addressable word array. Unlike memory, which is volatile, storage is non volatile and is maintained as part of the system state. All locations in both storage and memory are well-defined initially as zero.
The machine does not follow the standard von Neu- mann architecture. Rather than storing program code in generally-accessible memory or storage, it is stored sepa- rately in a virtual ROM interactable only through a spe- cialised instruction.
The machine can have exceptional execution for several reasons, including stack underflows and invalid instruc- tions. Like the out-of-gas exception, they do not leave state changes intact. Rather, the machine halts immedi- ately and reports the issue to the execution agent (either the transaction processor or, recursively, the spawning ex- ecution environment) which will deal with it separately.

### 9.2. Fees Overview.

Fees (denominated in gas) are charged under three distinct circumstances, all three as prerequisite to the execution of an operation. The first and most common is the fee intrinsic to the computation of the operation (see Appendix G). Secondly, gas may be deducted in order to form the payment for a subordinate message call or contract creation; this forms part of the payment for CREATE, CALL and CALLCODE. Finally, gas may be paid due to an increase in the usage of the memory.
Over an account’s execution, the total fee for memory- usage payable is proportional to smallest multiple of 32 bytes that are required such that all memory indices (whether for read or write) are included in the range. This is paid for on a just-in-time basis; as such, referencing an area of memory at least 32 bytes greater than any previ- ously indexed memory will certainly result in an additional memory usage fee. Due to this fee it is highly unlikely addresses will ever go above 32-bit bounds. That said, implementations must be able to manage this eventuality.
Storage fees have a slightly nuanced behaviour—to in- centivise minimisation of the use of storage (which corre- sponds directly to a larger state database on all nodes), the execution fee for an operation that clears an entry in the storage is not only waived, a qualified refund is given; in fact, this refund is effectively paid up-front since the initial usage of a storage location costs substantially more than normal usage.
See Appendix H for a rigorous definition of the EVM gas cost.

### 9.3. Execution Environment.

In addition to the sys- tem state σ, and the remaining gas for computation g, there are several pieces of important information used in the execution environment that the execution agent must provide; these are contained in the tuple I:
• Ia,theaddressoftheaccountwhichownsthecode that is executing.
• Io,thesenderaddressofthetransactionthatorig- inated this execution.
• Ip, the price of gas in the transaction that origi- nated this execution.
• Id, the byte array that is the input data to this execution; if the execution agent is a transaction, this would be the transaction data.
• Is, the address of the account which caused the code to be executing; if the execution agent is a transaction, this would be the transaction sender.
• Iv, the value, in Wei, passed to this account as part of the same procedure as execution; if the execution agent is a transaction, this would be the transaction value.
• Ib, the byte array that is the machine code to be executed.
• IH, the block header of the present block.
• Ie, the depth of the present message-call or contract-creation (i.e. the number of CALLs or
CREATEs being executed at present).
• Iw, the permission to make modifications to the
state.
The execution model defines the function Ξ, which can compute the resultant state σ′, the remaining gas g′, the accrued substate A and the resultant output, o, given these definitions. For the present context, we will define it as:
(128) (σ′, g′, A, o) ≡ Ξ(σ, g, I)
where we will remember that A, the accrued substate is defined as the tuple of the suicides set s, the log series l, the touched accounts t and the refunds r:
(129) A ≡ (s,l,t,r)

### 9.4. Execution Overview.

We must now define the Ξ function. In most practical implementations this will be modelled as an iterative progression of the pair comprising the full system state, σ and the machine state, μ. For- mally, we define it recursively with a function X. This uses an iterator function O (which defines the result of a single cycle of the state machine) together with functions Z which determines if the present state is an exceptional halting state of the machine and H, specifying the output data of the instruction if and only if the present state is a normal halting state of the machine.
The empty sequence, denoted (), is not equal to the empty set, denoted ∅; this is important when interpreting the output of H, which evaluates to ∅ when execution is to continue but a series (potentially empty) when execution

 ETHEREUM: A SECURE DECENTRALISED GENERALISED TRANSACTION LEDGER BYZANTIUM VERSION 56c4578 - 2018-02-0212
should halt. (130)
9.4.2. Exceptional Halting. The exceptional halting func- tion Z is defined as:
(131) (132) (133) (134) (135) (136) (137)
(σ′, μ′, A, ..., o)
μg
μpc μm μi μs μo
(138)
X (σ, μ, A, I)  ≡   0    ∅,μ,A ,I,∅      ∅,μ′,A0,I,o if Z(σ,μ,I) where (145) W (w, μ) ≡ where (139) (140) (141) (142) X O(σ, μ, A, I)
o ≡ H(μ,I)
Ξ(σ,g,I,T)
≡ (σ′,μ′g,A,o)
≡ X (σ, μ, A0, I)  ≡ g
≡ 0
≡ (0, 0, ...)
≡ 0
≡ ()
≡ ()
Z(σ, μ, I) ≡
if O(σ,μ,A,I)·o if o̸=∅
This states that the execution is in an exceptional halt- ing state if there is insufficient gas, if the instruction is in- valid (and therefore its δ subscript is undefined), if there are insufficient stack items, if a JUMP/JUMPI destination is invalid, the new stack size would be larger than 1024 or state modification is attempted during a static call. The astute reader will realise that this implies that no instruc- tion can, through its execution, cause an exceptional halt.

#### 9.4.3. Jump Destination Validity.

We previously used D as the function to determine the set of valid jump desti- nations given the code that is being run. We define this as any position in the code occupied by a JUMPDEST in- struction.
All such positions must be on valid instruction bound- aries, rather than sitting in the data portion of PUSH operations and must appear within the explicitly defined portion of the code (rather than in the implicitly defined STOP operations that trail it).
(a,b,c,d) · e ≡ μ′ ≡ μ′g ≡
(a,b,c,d,e)
μ except:
μg −C(σ,μ,I)
Note that, when we evaluate Ξ, we drop the fourth element I′ and extract the remaining gas μ′g from the re- sultant machine state μ′.
X is thus cycled (recursively here, but implementations are generally expected to use a simple iterative loop) until either Z becomes true indicating that the present state is exceptional and that the machine must be halted and any changes discarded or until H becomes a series (rather than the empty set) indicating that the machine has reached a controlled halt.

#### 9.4.1. Machine State.

The machine state μ is defined as the tuple (g, pc, m, i, s) which are the gas available, the program counter pc ∈ P256 , the memory contents, the ac- tive number of words in memory (counting continuously from position 0), and the stack contents. The memory contents μm are a series of zeroes of size 2256.
For the ease of reading, the instruction mnemonics, written in small-caps (e.g. ADD), should be interpreted as their numeric equivalents; the full table of instructions and their specifics is given in Appendix H.
For the purposes of defining Z, H and O, we define w as the current operation to be executed:
(143) w ≡
Formally: (146)
where: (147)
D(c) ≡ DJ (c, 0)
 Ib[μpc]
if μpc < ∥Ib∥ STOP otherwise
HRETURN (μ)

We also assume the fixed amounts of δ and α, specify- ing the stack items removed and added, both subscript- able on the instruction and an instruction cost function C evaluating to the full cost, in gas, of executing the given instruction.
w=REVERT otherwise
(144)
μg<C(σ,μ,I) ∨ δw=∅∨
∥μs∥ < δw ∨
(w ∈ {JUMP, JUMPI} ∧
μs[0] ∈/ D(Ib)) ∨
(w = RETURNDATACOPY∧
μs[1] + μs[2] > ∥μo∥) ∨ ∥μs∥−δw+αw>1024 ∨ ¬Iw∧W(w,μ)
w ∈ {CREATE, SSTORE, SELFDESTRUCT} ∨ LOG0≤w∧w≤LOG4 ∨
w ∈ {CALL, CALLCODE} ∧ μs [2] ̸= 0
{} 
if i |c|
DJ(c,i)≡ {i}∪DJ(c,N(i,c[i])) if c[i]=JUMPDEST
DJ (c, N (i, c[i])) otherwise
where N is the next valid instruction position in the code, skipping the data of a PUSH instruction, if any: (148)
N(i,w)≡

#### 9.4.4. Normal Halting.

The normal halting function H is defined:
(149)
 i + w − PUSH1 + 2
i + 1 otherwise
if w ∈ {RETURN, REVERT}
if w ∈ {STOP,SELFDESTRUCT}
The data-returning halt operations, RETURN and REVERT, have a special function HRETURN, defined in Ap- pendix H.
H(μ,I) ≡
()
∅ otherwise
if w ∈ [PUSH1,PUSH32]

 ETHEREUM: A SECURE DECENTRALISED GENERALISED TRANSACTION LEDGER BYZANTIUM VERSION 56c4578 - 2018-02-0213

### 9.5. The Execution Cycle.

Stack items are added or removed from the left-most, lower-indexed portion of the series; all other items remain unchanged:
11.1. Ommer Validation. The validation of ommer headers means nothing more than verifying that each om- mer header is both a valid header and satisfies the rela- tion of Nth-generation ommer to the present block where N ≤ 6. The maximum of ommer headers is two. Formally:
(162) ∥BU∥ 2   V(U) ∧ k(U,P(BH)H,6) U∈BU
where k denotes the “is-kin” property: (163)
(150) (151) (152) (153)
O (σ, μ, A, I)  ≡ (σ′, μ′, A′, I) ∆ ≡ αw−δw
∥μ′s∥ ≡ ∥μs∥ + ∆ ∀x ∈ [αw, ∥μ′s∥) : μ′s[x] ≡ μs[x − ∆]
The gas is reduced by the instruction’s gas cost and for most instructions, the program counter increments on each cycle, for the three exceptions, we assume a function J, subscripted by one of two instructions, which evaluates to the according value:
false
(154) (155)
μ′g ≡ μ′pc ≡
μg −C(σ,μ,I) JJUMP(μ) if
JJUMPI(μ) if
N (μpc , w) otherwise
In general, we assume the memory, self-destruct set and system state don’t change:
(156) (157) (158) (159)
μ′m ≡ μm μ′i ≡ μi A′ ≡ A σ′ ≡ σ
However, instructions do typically alter one or several components of these values. Altered components listed by instruction are noted in Appendix H, alongside values for α and δ and a formal description of the gas requirements.

## 10. Blocktree to Blockchain

The canonical blockchain is a path from root to leaf through the entire block tree. In order to have consensus over which path it is, conceptually we identify the path that has had the most computation done upon it, or, the heaviest path. Clearly one factor that helps determine the heaviest path is the block number of the leaf, equivalent to the number of blocks, not counting the unmined genesis block, in the path. The longer the path, the greater the total mining effort that must have been done in order to arrive at the leaf. This is akin to existing schemes, such as that employed in Bitcoin-derived protocols.
Since a block header includes the difficulty, the header alone is enough to validate the computation done. Any block contributes toward the total computation or total difficulty of a chain.
Thus we define the total difficulty of block B recur- sively as:
(166)
w = JUMP
w = JUMPI
and s denotes the “is-sibling” property: (164)
s(U,H)≡(P(H)=P(U) ∧ H̸=U ∧ U∈/B(H)U) where B(H) is the block of the corresponding header H.
11.2. Transaction Validation. The given gasUsed must correspond faithfully to the transactions listed: BH g , the total gas used in the block, must be equal to the ac- cumulated gas used according to the final transaction:
(165) BH g = l(R)u
11.3. Reward Application. The application of rewards
to a block involves raising the balance of the accounts of
the beneficiary address of the block and each ommer by a
certain amount. We raise the block’s beneficiary account
by Rb; for each ommer, we raise the block’s beneficiary
by an additional 1 of the block reward and the benefi- 32
ciary of the ommer gets rewarded depending on the block number. Formally we define the function Ω:
k(U, H, n) ≡

s(U, H)
if n = 0  ∨k(U,P(H)H,n−1) otherwise
 Ω(B,σ) ≡
σ′ : σ′ = σ σ[BHc]b+(1+∥BU∥)Rb
(167)σ′[BH c]b (168) ∀U∈BU :
=
except: 32
 Bt ≡ Bt′+Bd
σ′[Uc] = (169) a′ ≡
(170) R ≡
If there are collisions of the beneficiary addresses be- tween ommers and the block (i.e. two ommers with the same beneficiary address or an ommer with the same bene- ficiary address as the present block), additions are applied cumulatively.
We define the block reward as 3 Ether: (171) Let Rb = 3 × 1018
(160) (161)
its parent block and Bd is its difficulty.

## 11. Block Finalisation

The process of finalising a block involves four stages:
(1) Validate (or, if mining, determine) ommers;
(2) validate (or, if mining, determine) transactions;
(3) apply rewards;
(4) verify (or, if mining, compute a valid) state and
nonce.
(σ[Uc]n, σ[Uc]b + R, σ[Uc]s, σ[Uc]c) 1
B′ ≡ P(BH)
As such given a block B, Bt is its total difficulty, B′ is
(1+8(Ui−BHi))Rb
 ∅ ifσ[Uc]=∅ ∧ R=0 a′ otherwise

 ETHEREUM: A SECURE DECENTRALISED GENERALISED TRANSACTION LEDGER BYZANTIUM VERSION 56c4578 - 2018-02-0214

### 11.4. State & Nonce Validation.

We may now define the function, Γ, that maps a block B to its initiation state: (172)

### 11.5. Mining Proof-of-Work.

The mining proof-of- work (PoW) exists as a cryptographically secure nonce that proves beyond reasonable doubt that a particular amount of computation has been expended in the deter- mination of some token value n. It is utilised to enforce the blockchain security by giving meaning and credence to the notion of difficulty (and, by extension, total dif- ficulty). However, since mining new blocks comes with an attached reward, the proof-of-work not only functions as a method of securing confidence that the blockchain will remain canonical into the future, but also as a wealth distribution mechanism.
For both reasons, there are two important goals of the proof-of-work function; firstly, it should be as accessible as possible to as many people as possible. The requirement of, or reward from, specialised and uncommon hardware should be minimised. This makes the distribution model as open as possible, and, ideally, makes the act of mining a simple swap from electricity to Ether at roughly the same rate for anyone around the world.
Secondly, it should not be possible to make super-linear profits, and especially not so with a high initial barrier. Such a mechanism allows a well-funded adversary to gain a troublesome amount of the network’s total mining power and as such gives them a super-linear reward (thus skew- ing distribution in their favour) as well as reducing the network security.
One plague of the Bitcoin world is ASICs. These are specialised pieces of compute hardware that exist only to do a single task. In Bitcoin’s case the task is the SHA256 hash function. While ASICs exist for a proof-of-work func- tion, both goals are placed in jeopardy. Because of this, a proof-of-work function that is ASIC-resistant (i.e. diffi- cult or economically inefficient to implement in specialised compute hardware) has been identified as the proverbial silver bullet.
Two directions exist for ASIC resistance; firstly make it sequential memory-hard, i.e. engineer the function such that the determination of the nonce requires a lot of mem- ory and bandwidth such that the memory cannot be used in parallel to discover multiple nonces simultaneously. The second is to make the type of computation it would need to do general-purpose; the meaning of “specialised hard- ware” for a general-purpose task set is, naturally, general purpose hardware and as such commodity desktop com- puters are likely to be pretty close to “specialised hard- ware” for the task. For Ethereum 1.0 we have chosen the first path.
More formally, the proof-of-work function takes the form of PoW:
(182)
Γ(B) ≡
 σ0
σi : TRIE(LS(σi)) = P(BH)Hr
if P(BH)=∅ otherwise
Here, TRIE(LS(σi)) means the hash of the root node of a trie of state σi; it is assumed that implementations will store this in the state database, trivial and efficient since the trie is by nature an immutable data structure.
And finally define Φ, the block transition function, which maps an incomplete block B to a complete block B′:
Φ(B) ≡ B′: B′=B∗ except: B′ = n: x 2256
(175) Bm′ = m with (x,m) = PoW(Bn∗ ,n,d)
(176) B∗ ≡ B except: B∗ = r(Π(Γ(B), B)) r
(173) (174)
n Hd
  With
As specified at the beginning of the present work, Π is
d being a dataset as specified in appendix J.
the state-transition function, which is defined in terms of Ω, the block finalisation function and Υ, the transaction- evaluation function, both now well-defined.
As previously detailed, R[n]z, R[n]l and R[n]u are the nth corresponding status code, logs and cumulative gas used after each transaction (R[n]b, the fourth component in the tuple, has already been defined in terms of the logs). We also define the nth state σ[n], which is defined sim- ply as the state resulting from applying the corresponding transaction to the state resulting from the previous trans- action (or the block’s initial state in the case of the first such transaction):
(177) σ[n] =
 Γ(B)
Υ(σ[n − 1], BT[n])
if n<0 otherwise
In the case of BR[n]u, we take a similar approach defin- ing each item as the gas used in evaluating the correspond- ing transaction summed with the previous item (or zero, if it is the first), giving us a running total:
0
(178) R[n]u = Υg(σ[n−1],BT[n])

+R[n − 1]u
For R[n]l, we utilise the Υl function that we conve-
niently defined in the transaction execution function.
l
(179) R[n]l = Υ (σ[n − 1], BT[n])
We define R[n]z in a similar manner. (180) R[n]z =Υz(σ[n−1],BT[n])
Finally, we define Π as the new state given the block reward function Ω applied to the final transaction’s resul- tant state, l(σ):
(181) Π(σ, B) ≡ Ω(B, l(σ))
Thus the complete block-transition mechanism, less PoW, the proof-of-work function is defined.
if n<0 otherwise
m = Hm ∧
n
2256 Hd
with (m,n) = PoW(Hn,Hn,d)
  Where Hn is the new block’s header but without the nonce and mix-hash components; Hn is the nonce of the header; d is a large data set needed to compute the mix- Hash and Hd is the new block’s difficulty value (i.e. the block difficulty from section 10). PoW is the proof-of-work function which evaluates to an array with the first item being the mixHash and the second item being a pseudo- random number cryptographically dependent on H and d. The underlying algorithm is called Ethash and is described below.

 ETHEREUM: A SECURE DECENTRALISED GENERALISED TRANSACTION LEDGER BYZANTIUM VERSION 56c4578 - 2018-02-0215

#### 11.5.1. Ethash.

Ethash is the PoW algorithm for Ethereum 1.0. It is the latest version of Dagger- Hashimoto, introduced by Buterin [2013b] and Dryja [2014], although it can no longer appropriately be called that since many of the original features of both algorithms have been drastically changed in the last month of research and development. The general route that the algorithm takes is as follows:
There exists a seed which can be computed for each block by scanning through the block headers up until that point. From the seed, one can compute a pseudorandom cache, Jcacheinit bytes in initial size. Light clients store the cache. From the cache, we can generate a dataset, Jdatasetinit bytes in initial size, with the property that each item in the dataset depends on only a small number of items from the cache. Full clients and miners store the dataset. The dataset grows linearly with time.
Mining involves grabbing random slices of the dataset and hashing them together. Verification can be done with low memory by using the cache to regenerate the specific pieces of the dataset that you need, so you only need to store the cache. The large dataset is updated once ev- ery Jepoch blocks, so the vast majority of a miner’s effort will be reading the dataset, not making changes to it. The mentioned parameters as well as the algorithm is explained in detail in appendix J.

## 12. Implementing Contracts

There are several patterns of contracts engineering that allow particular useful behaviours; two of these that I will briefly discuss are data feeds and random numbers.
12.1. Data Feeds. A data feed contract is one which pro- vides a single service: it gives access to information from the external world within Ethereum. The accuracy and timeliness of this information is not guaranteed and it is the task of a secondary contract author—the contract that utilises the data feed—to determine how much trust can be placed in any single data feed.
The general pattern involves a single contract within Ethereum which, when given a message call, replies with some timely information concerning an external phenom- enon. An example might be the local temperature of New York City. This would be implemented as a contract that returned that value of some known point in storage. Of course this point in storage must be maintained with the correct such temperature, and thus the second part of the pattern would be for an external server to run an Ethereum node, and immediately on discovery of a new block, creates a new valid transaction, sent to the contract, updating said value in storage. The contract’s code would accept such updates only from the identity contained on said server.
12.2. Random Numbers. Providing random numbers within a deterministic system is, naturally, an impossible task. However, we can approximate with pseudo-random numbers by utilising data which is generally unknowable at the time of transacting. Such data might include the block’s hash, the block’s timestamp and the block’s benefi- ciary address. In order to make it hard for malicious miner to control those values, one should use the BLOCKHASH operation in order to use hashes of the previous 256 blocks as pseudo-random numbers. For a series of such numbers,
a trivial solution would be to add some constant amount and hashing the result.

## 13. Future Directions

The state database won’t be forced to maintain all past state trie structures into the future. It should maintain an age for each node and eventually discard nodes that are neither recent enough nor checkpoints; checkpoints, or a set of nodes in the database that allow a particular block’s state trie to be traversed, could be used to place a maxi- mum limit on the amount of computation needed in order to retrieve any state throughout the blockchain.
Blockchain consolidation could be used in order to re- duce the amount of blocks a client would need to download to act as a full, mining, node. A compressed archive of the trie structure at given points in time (perhaps one in every 10,000th block) could be maintained by the peer network, effectively recasting the genesis block. This would reduce the amount to be downloaded to a single archive plus a hard maximum limit of blocks.
Finally, blockchain compression could perhaps be con- ducted: nodes in state trie that haven’t sent/received a transaction in some constant amount of blocks could be thrown out, reducing both Ether-leakage and the growth of the state database.
13.1. Scalability. Scalability remains an eternal con- cern. With a generalised state transition function, it be- comes difficult to partition and parallelise transactions to apply the divide-and-conquer strategy. Unaddressed, the dynamic value-range of the system remains essentially fixed and as the average transaction value increases, the less valuable of them become ignored, being economically pointless to include in the main ledger. However, several strategies exist that may potentially be exploited to pro- vide a considerably more scalable protocol.
Some form of hierarchical structure, achieved by ei- ther consolidating smaller lighter-weight chains into the main block or building the main block through the in- cremental combination and adhesion (through proof-of- work) of smaller transaction sets may allow parallelisa- tion of transaction combination and block-building. Par- allelism could also come from a prioritised set of parallel blockchains, consolidated each block and with duplicate or invalid transactions thrown out accordingly.
Finally, verifiable computation, if made generally avail- able and efficient enough, may provide a route to allow the proof-of-work to be the verification of final state.

## 14. Conclusion

I have introduced, discussed and formally defined the protocol of Ethereum. Through this protocol the reader may implement a node on the Ethereum network and join others in a decentralised secure social operating system. Contracts may be authored in order to algorithmically specify and autonomously enforce rules of interaction.

### 15. Acknowledgements

Many thanks to Aeron Buchanan for authoring the Homestead revisions, Christoph Jentzsch for authoring the Ethash algorithm and Yoichi Hirai for doing most of the

 ETHEREUM: A SECURE DECENTRALISED GENERALISED TRANSACTION LEDGER BYZANTIUM VERSION 56c4578 - 2018-02-0216
EIP-150 changes. Important maintenance, useful correc- tions and suggestions were provided by a number of oth- ers from the Ethereum DEV organisation and Ethereum community at large including Gustav Simonsson, Pawel  Bylica,JuttaSteiner,NickSavers,ViktorTro ́n,Marko Simovic, Giacomo Tazzari and, of course, Vitalik Buterin.

### 16. Availability

The source of this paper is maintained at https: //github.com/ethereum/yellowpaper/. An auto- generated PDF is located at https://ethereum.github. io/yellowpaper/paper.pdf.
References
Jacob Aron. BitCoin software finds new life. New Scien- tist, 213(2847):20, 2012.
Adam Back. Hashcash - Amortizable Publicly Auditable Cost-Functions. 2002. URL http://www.hashcash. org/papers/amortizable.pdf.
Roman Boutellier and Mareike Heinzen. Pirates, Pioneers, Innovators and Imitators. In Growth Through Innova- tion, pages 85–96. Springer, 2014.
Vitalik Buterin. Ethereum: A Next-Generation Smart Contract and Decentralized Application Platform. 2013a. URL https://github.com/ethereum/wiki/ wiki/White-Paper.
Vitalik Buterin. Dagger: A Memory-Hard to Compute, Memory-Easy to Verify Scrypt Alternative. 2013b. URL http://vitalik.ca/ethereum/dagger.html.
Vitalik Buterin. Eip-2: Homestead hard-fork changes, 2015. URL https://github.com/ethereum/EIPs/ blob/master/EIPS/eip-2.md.
Vitalik Buterin. Eip-100: Change difficulty adjust- ment to target mean block time including uncles, April 2016. URL https://github.com/ethereum/ EIPs/blob/master/EIPS/eip-100.md.
B.A. Davey and H.A. Priestley. Introduction to lattices and order. 2nd ed. Cambridge: Cambridge University Press, 2nd ed. edition, 2002. ISBN 0-521-78451-4/pbk.
Thaddeus Dryja. Hashimoto: I/O bound proof of work. 2014. URL https://mirrorx.com/files/hashimoto. pdf.
Cynthia Dwork and Moni Naor. Pricing via processing or combatting junk mail. In In 12th Annual International Cryptology Conference, pages 139–147, 1992.
Phong Vo Glenn Fowler, Landon Curt Noll. FowlerNollVo hash function. 1991. URL https://en.wikipedia.org/wiki/Fowler%E2%80% 93Noll%E2%80%93Vo_hash_function#cite_note-2.
Nils Gura, Arun Patel, Arvinderpal Wander, Hans Eberle, and Sheueling Chang Shantz. Comparing elliptic curve cryptography and RSA on 8-bit CPUs. In Crypto- graphic Hardware and Embedded Systems-CHES 2004, pages 119–132. Springer, 2004.
Sergio Demian Lerner. Strict Memory Hard Hashing Func- tions. 2014. URL http://www.hashcash.org/papers/ memohash.pdf.
Mark Miller. The Future of Law. In paper delivered at the Extro 3 Conference (August 9), 1997.
Satoshi Nakamoto. Bitcoin: A peer-to-peer electronic cash system. Consulted, 1:2012, 2008.
Meni Rosenfeld. Overview of Colored Coins. 2012. URL https://bitcoil.co.il/BitcoinX.pdf.
Afri Schoedon and Vitalik Buterin. Eip-649: Metropo- lis difficulty bomb delay and block reward reduction, June 2017. URL https://github.com/ethereum/EIPs/ blob/master/EIPS/eip-649.md.
Yonatan Sompolinsky and Aviv Zohar. Accel- erating Bitcoin’s Transaction Processing. Fast Money Grows on Trees, Not Chains, 2013. URL CryptologyePrintArchive,Report2013/881. http://eprint.iacr.org/.
Simon Sprankel. Technical Basis of Digital Currencies, 2013.
Nick Szabo. Formalizing and securing relationships on public networks. First Monday, 2(9), 1997.
Vivek Vishnumurthy, Sangeeth Chandrakumar, and Emin Gn Sirer. Karma: A secure economic framework for peer-to-peer resource sharing, 2003.
J. R. Willett. MasterCoin Complete Specification. 2013. URL https://github.com/mastercoin-MSC/spec.
Appendix A. Terminology
External Actor: A person or other entity able to interface to an Ethereum node, but external to the world of Ethereum. It can interact with Ethereum through depositing signed Transactions and inspecting the blockchain and associated state. Has one (or more) intrinsic Accounts.
Address: A 160-bit code used for identifying Accounts.
Account: Accounts have an intrinsic balance and transaction count maintained as part of the Ethereum state.
They also have some (possibly empty) EVM Code and a (possibly empty) Storage State associated with them. Though homogenous, it makes sense to distinguish between two practical types of account: those with empty associated EVM Code (thus the account balance is controlled, if at all, by some external entity) and those with non-empty associated EVM Code (thus the account represents an Autonomous Object). Each Account has a single Address that identifies it.
Transaction: A piece of data, signed by an External Actor. It represents either a Message or a new Autonomous Object. Transactions are recorded into each block of the blockchain.
Autonomous Object: A notional object existent only within the hypothetical state of Ethereum. Has an intrinsic address and thus an associated account; the account will have non-empty associated EVM Code. Incorporated only as the Storage State of that account.
Storage State: The information particular to a given Account that is maintained between the times that the Account’s associated EVM Code runs.

 ETHEREUM: A SECURE DECENTRALISED GENERALISED TRANSACTION LEDGER BYZANTIUM VERSION 56c4578 - 2018-02-0217
Message: Data (as a set of bytes) and Value (specified as Ether) that is passed between two Accounts, either through the deterministic operation of an Autonomous Object or the cryptographically secure signature of the Transaction.
Message Call: The act of passing a message from one Account to another. If the destination account is associated with non-empty EVM Code, then the VM will be started with the state of said Object and the Message acted upon. If the message sender is an Autonomous Object, then the Call passes any data returned from the VM operation.
Gas: The fundamental network cost unit. Paid for exclusively by Ether (as of PoC-4), which is converted freely to and from Gas as required. Gas does not exist outside of the internal Ethereum computation engine; its price is set by the Transaction and miners are free to ignore Transactions whose Gas price is too low.
Contract: Informal term used to mean both a piece of EVM Code that may be associated with an Account or an Autonomous Object.
Object: Synonym for Autonomous Object.
App: An end-user-visible application hosted in the Ethereum Browser.
Ethereum Browser: (aka Ethereum Reference Client) A cross-platform GUI of an interface similar to a simplified
browser (a la Chrome) that is able to host sandboxed applications whose backend is purely on the Ethereum
protocol.
Ethereum Virtual Machine: (aka EVM) The virtual machine that forms the key part of the execution model
for an Account’s associated EVM Code.
Ethereum Runtime Environment: (aka ERE) The environment which is provided to an Autonomous Object
executing in the EVM. Includes the EVM but also the structure of the world state on which the EVM relies for
certain I/O instructions including CALL & CREATE.
EVM Code: The bytecode that the EVM can natively execute. Used to formally specify the meaning and rami-
fications of a message to an Account.
EVM Assembly: The human-readable form of EVM-code.
LLL: TheLisp-likeLow-levelLanguage,ahuman-writablelanguageusedforauthoringsimplecontractsandgeneral
low-level language toolkit for trans-compiling to.
Appendix B. Recursive Length Prefix
This is a serialisation method for encoding arbitrarily structured binary data (byte arrays).
We define the set of possible structures T:
(183) T ≡ L∪B
(184) L ≡ {t : t = (t[0],t[1],...) ∧ ∀n<∥t∥ t[n] ∈ T}
(185) B ≡ {b : b = (b[0],b[1],...) ∧ ∀n<∥b∥ b[n] ∈ O}
Where O is the set of bytes. Thus B is the set of all sequences of bytes (otherwise known as byte-arrays, and a leaf if imagined as a tree), L is the set of all tree-like (sub-)structures that are not a single leaf (a branch node if imagined as a tree) and T is the set of all byte-arrays and such structural sequences.
We define the RLP function as RLP through two sub-functions, the first handling the instance when the value is a byte array, the second when it is a sequence of further values:
• If the byte-array contains solely a single byte and that single byte is less than 128, then the input is exactly equal to the output.
• If the byte-array contains fewer than 56 bytes, then the output is equal to the input prefixed by the byte equal to the length of the byte array plus 128.
• Otherwise, the output is equal to the input prefixed by the minimal-length byte-array which when interpreted as a big-endian integer is equal to the length of the input byte array, which is itself prefixed by the number of bytes required to faithfully encode this length value plus 183.
(186) RLP(x) ≡
If the value to be serialised is a byte-array, the RLP serialisation takes one of three forms:
Formally, we define Rb: (187)
x 
Rb(x) ≡ (128+∥x∥)·x
 183 +  BE(∥x∥)   · BE(∥x∥) · x
n<∥b∥
if ∥x∥=1∧x[0]<128 elseif ∥x∥<56 otherwise
(188) (189)
BE(x) ≡ (b0,b1,...) : b0 ̸= 0 ∧ x =   bn · 256∥b∥−1−n n=0
(a) · (b,c) · (d,e) = (a,b,c,d,e)
 Rb(x) if x ∈ B Rl (x) otherwise

 ETHEREUM: A SECURE DECENTRALISED GENERALISED TRANSACTION LEDGER BYZANTIUM VERSION 56c4578 - 2018-02-0218
Thus BE is the function that expands a positive integer value to a big-endian byte array of minimal length and the dot operator performs sequence concatenation.
If instead, the value to be serialised is a sequence of other items then the RLP serialisation takes one of two forms:
• If the concatenated serialisations of each contained item is less than 56 bytes in length, then the output is equal to that concatenation prefixed by the byte equal to the length of this byte array plus 192.
• Otherwise, the output is equal to the concatenated serialisations prefixed by the minimal-length byte-array which when interpreted as a big-endian integer is equal to the length of the concatenated serialisations byte array, which is itself prefixed by the number of bytes required to faithfully encode this length value plus 247.
Thus we finish by formally defining Rl:
 (192 + ∥s(x)∥) · s(x) if ∥s(x)∥ < 56 (190) Rl(x) ≡  247 +  BE(∥s(x)∥)   · BE(∥s(x)∥) · s(x) otherwise
(191) s(x) ≡ RLP(x0) · RLP(x1)...
If RLP is used to encode a scalar, defined only as a positive integer (P or any x for Px), it must be specified as the shortest byte array such that the big-endian interpretation of it is equal. Thus the RLP of some positive integer i is defined as:
(192) RLP(i : i ∈ P) ≡ RLP(BE(i))
When interpreting RLP data, if an expected fragment is decoded as a scalar and leading zeroes are found in the byte sequence, clients are required to consider it non-canonical and treat it in the same manner as otherwise invalid RLP data, dismissing it completely.
There is no specific canonical encoding format for signed or floating-point values.
Appendix C. Hex-Prefix Encoding
Hex-prefix encoding is an efficient method of encoding an arbitrary number of nibbles as a byte array. It is able to store an additional flag which, when used in the context of the trie (the only context in which it is used), disambiguates between node types.
It is defined as the function HP which maps from a sequence of nibbles (represented by the set Y) together with a boolean value to a sequence of bytes (represented by the set B):
(193)
(194)
HP(x,t) : x ∈ Y ≡ f(t) ≡
if ∥x∥ is even
 (16f (t), 16x[0] + x[1], 16x[2] + x[3], ...)
(16(f (t) + 1) + x[0], 16x[1] + x[2], 16x[3] + x[4], ...) otherwise
 2 if t̸=0 0 otherwise
Thus the high nibble of the first byte contains two flags; the lowest bit encoding the oddness of the length and the second-lowest encoding the flag t. The low nibble of the first byte is zero in the case of an even number of nibbles and the first nibble in the case of an odd number. All remaining nibbles (now an even number) fit properly into the remaining bytes.
Appendix D. Modified Merkle Patricia Tree
The modified Merkle Patricia tree (trie) provides a persistent data structure to map between arbitrary-length binary data (byte arrays). It is defined in terms of a mutable data structure to map between 256-bit binary fragments and arbitrary-length binary data, typically implemented as a database. The core of the trie, and its sole requirement in terms of the protocol specification is to provide a single value that identifies a given set of key-value pairs, which may be either a 32 byte sequence or the empty byte sequence. It is left as an implementation consideration to store and maintain the structure of the trie in a manner that allows effective and efficient realisation of the protocol.
Formally, we assume the input value I, a set containing pairs of byte sequences: (195) I = {(k0 ∈ B,v0 ∈ B),(k1 ∈ B,v1 ∈ B),...}
When considering such a sequence, we use the common numeric subscript notation to refer to a tuple’s key or value, thus:
(196) ∀I∈II ≡ (I0,I1)
Any series of bytes may also trivially be viewed as a series of nibbles, given an endian-specific notation; here we
assume big-endian. Thus:
(197) y(I) = {(k′0 ∈ Y,v0 ∈ B),(k′1 ∈ Y,v1 ∈ B),...}
 ⌊kn[i÷2]÷16⌋ if i is even (198) ∀n ∀i:i<2∥kn∥ k′n[i] ≡ kn[⌊i ÷ 2⌋] mod 16 otherwise

 ETHEREUM: A SECURE DECENTRALISED GENERALISED TRANSACTION LEDGER BYZANTIUM VERSION 56c4578 - 2018-02-0219
We define the function TRIE, which evaluates to the root of the trie that represents this set when encoded in this structure:
(199) TRIE(I) ≡ KEC(c(I, 0))
We also assume a function n, the trie’s node cap function. When composing a node, we use RLP to encode the structure. As a means of reducing storage complexity, for nodes whose composed RLP is fewer than 32 bytes, we store the RLP directly; for those larger we assert prescience of the byte array whose Keccak hash evaluates to our reference. Thus we define in terms of c, the node composition function:
() 
(200) n(I, i) ≡ c(I, i) KEC(c(I, i))
if I=∅
if ∥c(I, i)∥ < 32 otherwise
In a manner similar to a radix tree, when the trie is traversed from root to leaf, one may build a single key-value pair. The key is accumulated through the traversal, acquiring a single nibble from each branch node (just as with a radix tree). Unlike a radix tree, in the case of multiple keys sharing the same prefix or in the case of a single key having a unique suffix, two optimising nodes are provided. Thus while traversing, one may potentially acquire multiple nibbles from each of the other two node types, extension and leaf. There are three kinds of nodes in the trie:
Leaf: A two-item structure whose first item corresponds to the nibbles in the key not already accounted for by the accumulation of keys and branches traversed from the root. The hex-prefix encoding method is used and the second parameter to the function is required to be true.
Extension: A two-item structure whose first item corresponds to a series of nibbles of size greater than one that are shared by at least two distinct keys past the accumulation of the keys of nibbles and the keys of branches as traversed from the root. The hex-prefix encoding method is used and the second parameter to the function is required to be false.
Branch: A 17-item structure whose first sixteen items correspond to each of the sixteen possible nibble values for the keys at this point in their traversal. The 17th item is used in the case of this being a terminator node and thus a key being ended at this point in its traversal.
A branch is then only used when necessary; no branch nodes may exist that contain only a single non-zero entry. We may formally define this structure with the structural composition function c:
(201)
RLP  HP(I [i..(∥I ∥ − 1)],true),I    if ∥I∥ = 1 where ∃I : I ∈ I 00 1

RLP HP(I [i..(j−1)],false),n(I,j) if i̸=j wherej=argmax :∃l:∥l∥=x:∀ :I [0..(x−1)]=l


c(I, i) ≡
0 xI∈I0
RLP (u(0), u(1), ..., u(15), v)  
   
u(j) ≡ v=10
otherwise
where
n({I : I ∈ I ∧ I0[i] = j}, i + 1)
D.1. Trie Database. Thus no explicit assumptions are made concerning what data is stored and what is not, since that is an implementation-specific consideration; we simply define the identity function mapping the key-value set I to a 32-byte hash and assert that only a single such hash exists for any I, which though not strictly true is accurate within acceptable precision given the Keccak hash’s collision resistance. In reality, a sensible implementation will not fully recompute the trie root hash for each set.
A reasonable implementation will maintain a database of nodes determined from the computation of various tries or, more formally, it will memoise the function c. This strategy uses the nature of the trie to both easily recall the contents of any previous key-value set and to store multiple such sets in a very efficient manner. Due to the dependency relationship, Merkle-proofs may be constructed with an O(logN) space requirement that can demonstrate a particular leaf must exist within a trie of a given root hash.
Appendix E. Precompiled Contracts
For each precompiled contract, we make use of a template function, ΞPRE, which implements the out-of-gas checking.
 (∅,0,A0,()) if g<gr (202) ΞPRE(σ, g, I, T ) ≡ (σ, g − gr, A0, o) otherwise
The precompiled contracts each use these definitions and provide specifications for the o (the output data) and gr, the gas requirements.
For the elliptic curve DSA recover VM execution function, we also define d to be the input data, well-defined for an infinite length by appending zeroes as required. Importantly in the case of an invalid signature (ECDSARECOVER(h, v, r, s) =
I if ∃I:I∈I∧∥I∥=i () otherwise

 ETHEREUM: A SECURE DECENTRALISED GENERALISED TRANSACTION LEDGER BYZANTIUM VERSION 56c4578 - 2018-02-0220
∅), then we have no output.
(203) (204)
(205)
(206) (207) (208) (209) (210) (211) (212) (213) (214)
≡
ΞPRE where:
3000
 0 if ECDSARECOVER(h, v, r, s) = ∅ 32 otherwise
0
KEC ECDSARECOVER(h, v, r, s) [12..31] Id
(0, 0, ...)
d[0..31]
d[32..63]
d[64..95]
d[96..127]
Their
(215) (216)
(217) (218)
(219)
(220) (221) (222)
≡ ΞPRE where: = 60 + 12 |Id|
ΞECREC gr
if |o|=32: o[0..11] o[12..31] d[0..(|Id | − 1)] d[|Id |..] h v r s
= |o| =
= = = = = = = =
where:
SHA2-256 are more trivially defined as an almost pass-through operation. gas usage is dependent on the input data size, a factor rounded up to the nearest number of words.
ΞSHA256
gr
o[0..31]
ΞRIP160
gr
o[0..11] o[12..31]
The two hash functions, RIPEMD-160 and
 32 = SHA256(Id )
≡ ΞPRE where:
= 600 + 120 |Id|
 32 = RIPEMD160(Id )
=0
For the purposes here, we assume we have well-defined standard cryptographic functions for RIPEMD-160 and SHA2- 256 of the form:
(223) SHA256(i ∈ B) ≡ o ∈ B32 (224) RIPEMD160(i ∈ B) ≡ o ∈ B20
The fourth contract, the identity function ΞID simply defines the output as the input:
ΞID ≡ ΞPRE where: gr = 15+3 |Id|
32
o = Id
The fifth contract performs arbitrary-precision exponentiation under modulo. Here, 00 is taken to be one, and x mod 0 is zero for all x. The first word in the input specifies the number of bytes that the first non-negative integer B occupies. The second word in the input specifies the number of bytes that the second non-negative integer E occupies. The third word in the input specifies the number of bytes that the third non-negative integer M occupies. These three words are followed by B, E and M. The rest of the input is discarded. Whenever the input is too short, the missing bytes are considered to be zero. The output is encoded big-endian into the same format as M’s.
(225) (226) (227)

 ETHEREUM: A SECURE DECENTRALISED GENERALISED TRANSACTION LEDGER
BYZANTIUM VERSION 56c4578 - 2018-02-0221
(228) (229)
(230)
(231)
(232) (233) (234) (235) (236) (237) (238)
ΞEXPMOD ≡ gr =
f (x)
l′E o
lB lE lM B E M
ΞPRE except:  f max(lM,lB) max(l′E,1)
 Gquaddivisor   x2   + 96x − 3072
x2
if x ≤ 64
if 64 < x ≤ 1024
otherwise
≡ 4   2
  x 16
+ 480x − 199680 0
⌊log (E)⌋ = 2
if lE ≤ 32 ∧ E = 0
iflE ≤32∧E̸=0
if 32 < lE ∧ i[(96 + lB)..(127 + lB)] ̸= 0 otherwise
 8(lE − 32) + ⌊log2(i[(96 + lB)..(127 + lB)])⌋ 8(lE − 32)
= (BE mod M) ∈ P8lM ≡ i[0..31]
≡ i[32..63]
≡ i[64..95]
≡ i[96..(95 + lB )]
≡ i[(96 + lB)..(95 + lB + lE)]
≡ i[(96+lB +lE)..(95+lB +lE +lM)]
 Id[x] if x < |Id| 0 otherwise
i[x] ≡
(240) p ≡ 21888242871839275222246405745257275088696311157297823662689037894645226208583
(241) q ≡ 21888242871839275222246405745257275088548364400416034343698204186575808495617
Since p is a prime number, {0, 1, . . . , p − 1} forms a field with addition and multiplication modulo p. We call this field Fp.
We define a set C1 with
(242) C1 ≡ {(X,Y) ∈ Fp ×Fp | Y2 = X3 +3}∪{(0,0)} We define a binary operation + on C1 with
(239)
E.1. zkSNARK Related Precompiled Contracts. We choose two numbers, both of which are prime.
(X1, Y1) + (X2, Y2) ≡
 (X,Y) ifX1̸=X2 (0, 0) otherwise
(243)
(244) (245)
λ ≡
(C1,+) is known to form a group. We define the scalar multiplication · with
(247) n·P ≡(0,0)+P +···+P
n
for a natural number n and a point P in C1.
We define P1 to be a point (1,2) on C1. Let G1 be the subgroup of (C1,+) generated by P1. G1 is known to be a
cyclic group of order q. For a point P in G1, we define logP1(P) to be the smallest natural number n satisfying n·P1 = P. logP1 (P ) is at most q − 1.
Let Fp2 be a field Fp[i]/(i + 1). We define a set C2 with
(248) C2 ≡{(X,Y)∈Fp2 ×Fp2 |Y2 =X3 +3}∪{(0,0)}
We define a binary operation + and a scalar multiplication · with the same equations (243) and (247). (C2,+) is also known to be a group. We define P2 in C2 with
(249) P2 ≡ (11559732032986387107991004021392285783925812861821192530917403151452391805634 × i +10857046999023057135944570762232829481370756359578518086990519993285655852781, 4082367875863433681332203403145435568316851327593401208105741076214120093531 × i +8495653923123431417604973247489272438418190587263600148770280649306958101930)
(246)
Y2 − Y1 X2 − X1
X ≡ λ2−X1−X2
Y ≡ λ(X1 −X)−Y1

 ETHEREUM: A SECURE DECENTRALISED GENERALISED TRANSACTION LEDGER BYZANTIUM VERSION 56c4578 - 2018-02-0222
We define G2 to be the subgroup of (C2,+) generated by P2. G2 is known to be a cyclic group of order q. For a point P in G2, we define logP2(P) be the smallest natural number n satisfying n·P2 = P. With this definition, logP2(P) is at most q − 1.
(251)
(252)
(253) (254)
δ1 (x) ≡ g1 ≡
x ≡ y ≡
(255)
(256)
(257) (258) (259) (260)
δ2 (x) ≡ g2 ≡
x0 ≡ y0 ≡ x1 ≡ y1 ≡
 g2 if g2 ∈ G2 ∅ otherwise
(261) (262) (263)
(264) (265)
(266)
(267) (268) (269)
(270) (271)
ΞSNARKV ≡ ΞSNARKV(σ, g, I) = F ≡
k = gr =
o[0..31] ≡ v ≡
a1 ≡ b1 ≡
.
ak ≡ bk ≡
ΞPRE except:
(∅,0,A0,()) if F (|Id|mod192̸=0∨(∃j.aj =∅∨bj =∅))
|Id | 192
60000k + 40000  0x0000000000000000000000000000000000000000000000000000000000000001
0x0000000000000000000000000000000000000000000000000000000000000000 (logP1(a1)×logP2(b1)+···+logP1(ak)×logP2(bk)≡1 modq)
δ1 (Id [0..63]) δ2 (Id [64..191])
if v ∧ ¬F if ¬v ∧ ¬F
(272) (273) (274) (275) (276) (277)
(278)
ΞBN ADD ≡ ΞBN ADD(σ,g,I) = gr = o ≡ x ≡ y ≡
 ̄
Id [x] ≡
ΞBN PRE except:
(∅,0,A0,()) ifx=∅∨y=∅
500
A 32 byte number x ∈ P256 might and might not represent an element of Fp.
 g1 if g1 ∈ G1 ∅ otherwise
 (x,y) ifx̸=∅∧y̸=∅ ∅ otherwise
δp (x[0..31]) δp (x[32..63])
(250) δp(x) ≡
A 64 byte data x ∈ B512 might and might not represent an element of G1.
A 128 byte data x ∈ B1024 might and might not represent an element of G2.
 ((x0i+y0),(x1i+y1))
∅ otherwise
δp (x[0..31]) δp (x[32..63]) δp (x[64..95]) δp (x[96..127])
We define a precompiled contract for zkSNARK verification.
 x ifx<p ∅ otherwise
if x0 ̸= ∅∧y0 ̸= ∅∧x1 ̸= ∅∧y1 ̸= ∅
 We define a precompiled contract for
δ1 (Id [(|Id | − δ2 (Id [(|Id | −
192)..(|Id | − 129)]) 128)..(|Id | − 1)])
addition on G1.
   δ−1(x + y) 1
where + is the group operation in G1 δ1 (Id [64..127])
 Id[x] if x < |Id| 0 otherwise
 ̄
δ1 (Id [0..63])
 ̄

 ETHEREUM: A SECURE DECENTRALISED GENERALISED TRANSACTION LEDGER BYZANTIUM VERSION 56c4578 - 2018-02-0223
(279) (280) (281) (282) (283) (284)
ΞBN MUL ≡ ΞBN   MUL(σ, g, I) = gr = o ≡ n ≡ x ≡
ΞPRE except:
(∅,0,A0,()) if x = ∅
40000
(285) (286) (287)
ECDSAPUBKEY(pr ∈ B32) ≡ ECDSASIGN(e ∈ B32,pr ∈ B32) ≡ ECDSARECOVER(e ∈ B32,v ∈ B1,r ∈ B32,s ∈ B32) ≡
pu ∈ B64
(v ∈ B1,r ∈ B32,s ∈ B32) pu ∈ B64
 ̄
We define a precompiled contract for scalar multiplication on G1, where Id is defined in (278).
 δ−1(n · x) 1
where · is the scalar multiplication in G1 δ1 (Id [32..95])
 ̄
Id [0..31]
 ̄
Appendix F. Signing Transactions
The method of signing transactions is similar to the ‘Electrum style signatures’; it utilises the SECP-256k1 curve as described by Gura et al. [2004].
It is assumed that the sender has a valid private key pr, which is a randomly selected positive integer (represented as a byte array of length 32 in big-endian form) in the range [1, secp256k1n − 1].
We assert the functions ECDSASIGN, ECDSARECOVER and ECDSAPUBKEY. These are formally defined in the literature.
Where pu is the public key, assumed to be a byte array of size 64 (formed from the concatenation of two positive integers each < 2256) and pr is the private key, a byte array of size 32 (or a single positive integer in the aforementioned range). It is assumed that v is either the ‘recovery identifier’ or ‘chain identifier doubled plus 35 or 36’. The recovery identifier is a 1 byte value specifying the parity and finiteness of the coordinates of the curve point for which r is the x-value; this value is in the range of [27, 30], however we declare the upper two possibilities, representing infinite values, invalid. The value 27 represents an even y value and 28 represents an odd y value.
We declare that a signature is invalid unless all the following conditions are true:
(288) (289) (290)
where: (291)
0 < r < secp256k1n
0 < s < secp256k1n÷2+1
v ∈ {27,28,chain id × 2 + 35,chain id × 2 + 36}
secp256k1n = 115792089237316195423570985008687907852837564279074904382605163141518161494337
  For a given private key, pr, the Ethereum address A(pr) (a 160-bit value) to which it corresponds is defined as the right most 160-bits of the Keccak hash of the corresponding ECDSA public key:
(292) A(pr) = B96..255 KEC ECDSAPUBKEY(pr)
The message hash, h(T), to be signed is the Keccak hash of the transaction. Two different flavors of signing schemes are available. One operates without the latter three signature components, formally described as Tr, Ts and Tw. The other operates on nine elements:
 (Tn, Tp, Tg, Tt, Tv, p)
(Tn, Tp, Tg, Tt, Tv, p, chain id, (), ())
 Ti if Tt = 0 Td otherwise
if v ∈ {27, 28} otherwise
We may then define the sender function S of the transaction as:
(297) S (T ) ≡ B96..255  KEC ECDSARECOVER(h(T ), v0 , Tr , Ts )
(293)
LS(T) ≡ where
p≡
 h(T) ≡
(295) G(T,pr) ≡ T
(294)
The signed transaction G(T,pr) is defined as:
except: (296) (Tw,Tr,Ts) = ECDSASIGN(h(T),pr)
(298) v0 ≡
 Tw if Tw ∈ {27, 28} 28 − (Tw mod 2) otherwise
KEC(LS (T ))
The assertion that the sender of a signed transaction equals the address of the signer should be self-evident: (299) ∀T : ∀pr : S(G(T, pr)) ≡ A(pr)

 ETHEREUM: A SECURE DECENTRALISED GENERALISED TRANSACTION LEDGER BYZANTIUM VERSION 56c4578 - 2018-02-0224
Appendix G. Fee Schedule
The fee schedule G is a tuple of 31 scalar values corresponding to the relative costs, in gas, of a number of abstract operations that a transaction may effect.
 Name Value
Gzero 0 Gbase 2 Gverylow 3 Glow 5 Gmid 8 Ghigh 10 Gextcode 700 Gbalance 400 Gsload 200
Description*
Nothing paid for operations of the set Wzero.
Amount of gas to pay for operations of the set Wbase.
Amount of gas to pay for operations of the set Wverylow.
Amount of gas to pay for operations of the set Wlow.
Amount of gas to pay for operations of the set Wmid.
Amount of gas to pay for operations of the set Whigh.
Amount of gas to pay for operations of the set Wextcode.
Amount of gas to pay for a BALANCE operation.
Paid for a SLOAD operation.
Paid for a JUMPDEST operation.
Paid for an SSTORE operation when the storage value is set to non-zero from zero.
Paid for an SSTORE operation when the storage value’s zeroness remains unchanged or is set to zero. Refund given (added into refund counter) when the storage value is set to zero from non-zero. Refund given (added into refund counter) for self-destructing an account.
Amount of gas to pay for a SELFDESTRUCT operation.
Paid for a CREATE operation.
Paid per byte for a CREATE operation to succeed in placing code into state.
Paid for a CALL operation.
Paid for a non-zero value transfer as part of the CALL operation.
A stipend for the called contract subtracted from Gcallvalue for a non-zero value transfer.
Paid for a CALL or SELFDESTRUCT operation which creates an account.
Partial payment for an EXP operation.
Partial payment when multiplied by ⌈log256(exponent)⌉ for the EXP operation.
Paid for every additional word when expanding memory.
Paid by all contract-creating transactions after the Homestead transition.
Paid for every zero byte of data or code for a transaction.
Paid for every non-zero byte of data or code for a transaction.
Paid for every transaction.
Partial payment for a LOG operation.
Paid for each byte in a LOG operation’s data.
Paid for each topic of a LOG operation.
Paid for each SHA3 operation.
Paid for each word (rounded up) for input data to a SHA3 operation.
Partial payment for *COPY operations, multiplied by words copied, rounded up.
Payment for BLOCKHASH operation.
The quadratic coefficient of the input sizes of the exponation-over-modulo precompiled contract.
Appendix H. Virtual Machine Specification
When interpreting 256-bit binary values as integers, the representation is big-endian.
When a 256-bit machine datum is converted to and from a 160-bit address or hash, the rightwards (low-order for BE) 20 bytes are used and the left most 12 are discarded or filled with zeroes, thus the integer values (when the bytes are interpreted as big-endian) are equivalent.
 1 20000 5000 15000 24000 5000 32000 200 700 9000 2300 25000 10 50 3 32000 4 68 21000 Glog 375 Glogdata 8 Glogtopic 375 Gsha3 30 Gsha3word 6 Gcopy 3 Gblockhash 20 Gquaddivisor 100
Gj umpdest Gsset
Gsreset Rsclear
Rself destruct Gself destruct Gcreate Gcodedeposit Gcall Gcallvalue Gcallstipend Gnewaccount Gexp
Gexpbyte Gmemory Gtxcreate Gtxdatazero Gtxdatanonzero Gtransaction
 H.1. Gas Cost. The general gas cost function, C, is defined as:

 ETHEREUM: A SECURE DECENTRALISED GENERALISED TRANSACTION LEDGER
BYZANTIUM VERSION 56c4578 - 2018-02-0225
(300)
C(σ, μ, I) ≡ Cmem(μ′i)−Cmem(μi)+Gcreate 
(301)
where: (302)
if otherwise
CSSTORE (σ, μ) 

G
G +G ×(1+⌊log (μ[1])⌋)
 exp expbyte
 256s s 
 exp 
if w = SSTORE
if w=EXP∧μ [1]=0
s

Gverylow + Gcopy × ⌈μ [2] ÷ 32⌉
if w=EXP∧μ [1]>0
if w = CALLDATACOPY ∨ CODECOPY ∨ RETURND if w = EXTCODECOPY
if w=LOG0
if w=LOG1
if w=LOG2
if w=LOG3
if w=LOG4
if w=CALL ∨ CALLCODE ∨ DELEGATECALL
if w = SELFDESTRUCT
if w = CREATE
if w=SHA3
if w = JUMPDEST
if w = SLOAD
if w ∈ Wzero
if w∈W
if w∈W
if w∈W
if w∈W
if w ∈ Whigh
if w ∈ Wextcode
if w = BALANCE
if w = BLOCKHASH
s


Gextcode + Gcopy × ⌈μ [3] ÷ 32⌉ s
 
Glog + Glogdata × μ [1] s

Glog + Glogdata × μ [1] + Glogtopic s
Glog + Glogdata × μ [1] + 2Glogtopic  s

G +G ×μ[1]+3G

 log logdata s logtopic 

G +G ×μ[1]+4G
 log logdata s logtopic 

C (σ, μ)
 CALL 
C
 SELFDESTRUCT
(σ, μ)

Gsha3 + Gsha3word⌈s[1] ÷ 32⌉ 

Gj umpdest


Gsload
Gzero 
G
 base


G
 verylow 

G
 low

G
 mid 
base verylow low
mid
Ghigh 
 Gextcode 
 Gbalance
Gblockhash w ≡

 Ib[μpc] STOP
μpc < ∥Ib∥
Cmem(a) ≡ Gmemory · a +   a2   512
 with CCALL, CSELFDESTRUCT and CSSTORE as specified in the appropriate section below. We define the following subsets of instructions:
Wzero = {STOP, RETURN, REVERT}
Wbase = {ADDRESS, ORIGIN, CALLER, CALLVALUE, CALLDATASIZE, CODESIZE, GASPRICE, COINBASE,
TIMESTAMP, NUMBER, DIFFICULTY, GASLIMIT, RETURNDATASIZE, POP, PC, MSIZE, GAS} Wverylow = {ADD, SUB, NOT, LT, GT, SLT, SGT, EQ, ISZERO, AND, OR, XOR, BYTE, CALLDATALOAD,
MLOAD, MSTORE, MSTORE8, PUSH*, DUP*, SWAP*}
Wlow = {MUL, DIV, SDIV, MOD, SMOD, SIGNEXTEND}
Wmid = {ADDMOD, MULMOD, JUMP}
Whigh = {JUMPI}
Wextcode = {EXTCODESIZE}
Note the memory cost component, given as the product of Gmemory and the maximum of 0 & the ceiling of the number
of words in size that the memory must be over the current number of words, μi in order that all accesses reference valid memory whether for read or write. Such accesses must be for non-zero number of bytes.
Referencing a zero length range (e.g. by attempting to pass it as the input range to a CALL) does not require memory to be extended to the beginning of the range. μ′i is defined as this new maximum number of words of active memory; special-cases are given where these two are not equal.
Note also that Cmem is the memory cost function (the expansion function being the difference between the cost before and after). It is a polynomial, with the higher-order coefficient divided and floored, and thus linear up to 724B of memory used, after which it costs substantially more.
While defining the instruction set, we defined the memory-expansion for range function, M, thus:
(303) M(s,f,l) ≡
 s if l=0 max(s, ⌈(f + l) ÷ 32⌉) otherwise

 ETHEREUM: A SECURE DECENTRALISED GENERALISED TRANSACTION LEDGER BYZANTIUM VERSION 56c4578 - 2018-02-0226
Another useful function is “all but one 64th” function L defined as: (304) L(n) ≡ n − ⌊n/64⌋
H.2. Instruction Set. As previously specified in section 9, these definitions take place in the final context there. In particular we assume O is the EVM state-progression function and define the terms pertaining to the next cycle’s state (σ′, μ′) such that:
(305) O(σ, μ, A, I) ≡ (σ′, μ′, A′, I) with exceptions, as noted
Here given are the various exceptions to the state transition rules given in section 9 specified for each instruction, together with the additional instruction-specific definitions of J and C. For each instruction, also specified is α, the additional items placed on the stack and δ, the items removed from stack, as defined in section 9.
0s: Stop and Arithmetic Operations
All arithmetic is modulo 2256 unless otherwise noted. The zero-th power of zero 00 is defined to be one.
 Value Mnemonic δ α
Description
Halts execution. Addition operation.
μ′s[0] ≡ μs[0] + μs[1] Multiplication operation.
μ′s[0] ≡ μs[0] × μs[1] Subtraction operation.
μ′s[0] ≡ μs[0] − μs[1] Integer division operation.
 0
μ′s[0] ≡
⌊μs[0] ÷ μs[1]⌋
0x00 STOP
0x01 ADD
0x02 MUL
0x03 SUB
0x04 DIV
0x05 SDIV
0x06 MOD
0x07 SMOD
0x08 ADDMOD
0x09 MULMOD
0x0a EXP
0x0b SIGNEXTEND
0 0 2 1
2 1 2 1 2 1
2 1
2 1
2 1
3 1
3 1
2 1 2 1
if μ[1]=0 s
    otherwise
Signed integer division operation (truncated).
 0 if μ[1]=0 s
μ′s[0] ≡ −2255 if μs[0] = −2255 ∧ sgn(μs[0] ÷ μs[1])⌊|μs[0] ÷ μs[1]|⌋ otherwise
μs[1] = −1 Where all values are treated as two’s complement signed 256-bit integers.
Note the overflow semantic when −2255 is negated. Modulo remainder operation.
  0
μ′s[0] ≡
μs[0] mod μs[1]
if μ[1]=0 s
otherwise Signed modulo remainder operation.
  0
μ′s[0] ≡
sgn(μs[0])(|μs[0]| mod |μs[1]|)
if μ[1]=0 s
otherwise
Where all values are treated as two’s complement signed 256-bit integers.
 Modulo addition operation.
 0
μ′s[0] ≡
(μs[0] + μs[1])
if μ[2]=0 s
mod μs[2]
All intermediate calculations of this operation are not subject to the 2256 modulo.
otherwise
 Modulo multiplication operation.
 0
μ′s[0] ≡
(μs[0] × μs[1])
if μ[2]=0 s
Exponential operation. μ′s[0] ≡ μs[0]μs[1]
Extend length of two’s complement signed integer.
mod μs[2]
All intermediate calculations of this operation are not subject to the 2256 modulo.
otherwise
  μs[x]i gives the ith bit (counting from zero) of μs[x]
 μ [1]t ∀i ∈ [0..255] : μ′s[0]i ≡ s
if i   t where t = 256−8(μ [0]+1) s
otherwise
μs [1]i

 ETHEREUM: A SECURE DECENTRALISED GENERALISED TRANSACTION LEDGER BYZANTIUM VERSION 56c4578 - 2018-02-0227
 Value Mnemonic δ α
10s: Comparison & Bitwise Logic Operations Description
Less-than comparison.
0x10 LT
0x11 GT
0x12 SLT
0x13 SGT
0x14 EQ
0x15 ISZERO
0x16 AND
0x17 OR
0x18 XOR
0x19 NOT
0x1a BYTE
2 1
2 1
2 1
2 1
2 1
1 1
2 1 2 1 2 1 1 1
2 1
 1 if μ [0]<μ [1] s s
μ′s[0] ≡
Greater-than comparison.
0 otherwise
 1 if μ [0]>μ [1]
 μ′s[0] ≡
Signed less-than comparison.
0 otherwise
Signed greater-than comparison.
s s
0 otherwise
 1 if μ [0]<μ [1]
 μ′s[0] ≡
Where all values are treated as two’s complement signed 256-bit integers.
s s
  1 if μ [0]>μ [1] s s
μ′s[0] ≡
Where all values are treated as two’s complement signed 256-bit integers.
0 otherwise Equality comparison.
  1
μ′s[0]≡
0
Simple not
 1
μ′s[0]≡
0
if μ [0]=μ [1] ss
otherwise operator.
if μ[0]=0 s
 otherwise Bitwise AND operation.
 ∀i ∈ [0..255] : μ′s [0]i ≡ μs [0]i ∧ μs [1]i Bitwise OR operation.
 ∀i ∈ [0..255] : Bitwise XOR
∀i ∈ [0..255] : Bitwise NOT
μ′s [0]i ≡ μs [0]i ∨ μs [1]i operation.
μ′s [0]i ≡ μs [0]i ⊕ μs [1]i operation.
 1 if μ[0]i=0 μ′s[0]i ≡ s
  ∀i ∈ [0..255] :
Retrieve single byte from word.
0 otherwise
 μ [1] if
20s: SHA3
Description
Compute Keccak-256 hash.
μ′s[0] ≡ Keccak(μm[μs[0] . . . (μs[0] + μs[1] − 1)]) μ′i ≡M(μi,μs[0],μs[1])
 ∀i ∈ [0..255] : μ′s[0]i ≡ s (i+8μs[0]) 0
i<8∧μ [0]<32 s
otherwise
For Nth byte, we count from the left (i.e. N=0 would be the most significant in big endian).
  Value Mnemonic δ α 0x20 SHA3 21

 ETHEREUM: A SECURE DECENTRALISED GENERALISED TRANSACTION LEDGER BYZANTIUM VERSION 56c4578 - 2018-02-0228
 Value Mnemonic
0x30 ADDRESS
0x31 BALANCE
0x32 ORIGIN
0x33 CALLER
0x34 CALLVALUE
0x35 CALLDATALOAD
0x36 CALLDATASIZE
0x37 CALLDATACOPY
0x38 CODESIZE
0x39 CODECOPY
0x3a GASPRICE
0x3b EXTCODESIZE
0x3c EXTCODECOPY
0x3d RETURNDATASIZE
0x3e RETURNDATACOPY
30s: Environmental Information δα Description
0 1 Get address of currently executing account. μ′s[0] ≡ Ia
1 1 Get balance of the given account.
  σ[μ [0]]b μ′s[0] ≡ s
0
if σ[μ [0] mod 2160] ̸= ∅ s
otherwise
 0 1 Get execution origination address. μ′s[0] ≡ Io
This is the sender of original transaction; it is never an account with non-empty associated code.
0 1 Get caller address. μ′s[0] ≡ Is
This is the address of the account that is directly responsible for this execution. 0 1 Get deposited value by the instruction/transaction responsible for this execution.
μ′s[0] ≡ Iv
1 1 Get input data of current environment.
μ′s[0] ≡ Id[μs[0]...(μs[0] + 31)] with Id[x] = 0 if x   ∥Id∥
This pertains to the input data passed with the message call instruction or transaction.
0 1 Get size of input data in current environment. μ′s[0] ≡ ∥Id∥
This pertains to the input data passed with the message call instruction or transaction. 3 0 Copy input data in current environment to memory.
      Id[μ [1]+i] ∀i∈{0...μs[2]−1}μ′m[μs[0] + i] ≡ s
μ′i ≡M(μi,μs[0],μs[2])
This pertains to the input data passed with the message call instruction or transaction.
0 1 Get size of code running in current environment. μ′s[0] ≡ ∥Ib∥
3 0 Copy code running in current environment to memory.
0
The additions in μs[1] + i are not subject to the 2256 modulo.
μ′i ≡M(μi,μs[0],μs[2])
The additions in μs[1] + i are not subject to the 2256 modulo.
0 1 Get price of gas in current environment. μ′s[0] ≡ Ip
This is gas price specified by the originating transaction. 1 1 Get size of an account’s code.
if μ [1]+i < ∥Id∥ s
otherwise
   Ib[μ [1]+i] ∀i∈{0...μs[2]−1}μ′m[μs[0] + i] ≡ s
STOP
if μ [1]+i < ∥Ib∥ s
otherwise
  μ′s[0]≡∥σ[μs[0] mod2160]c∥
4 0 Copy an account’s code to memory.
 c[μ [2]+i] ∀i∈{0...μs[3]−1}μ′m[μs[1] + i] ≡ s
STOP
if μ [2]+i < ∥c∥ s
otherwise
 where c ≡ σ[μs[0] mod 2160]c
μ′i ≡M(μi,μs[1],μs[3])
The additions in μs[2] + i are not subject to the 2256 modulo.
0 1 Get size of output data from the previous call from the current environment. μ′s[0] ≡ ∥μo∥
  3 0 Copy output data from the previous call to memory.
 μ [μ [1]+i] if μ [1]+i<∥μ ∥
∀i∈{0...μs[2]−1}μ′m[μs[0] + i] ≡ o s s o 0 otherwise
The additions in μs[1] + i are not subject to the 2256 modulo. μ′i ≡M(μi,μs[0],μs[2])

 ETHEREUM: A SECURE DECENTRALISED GENERALISED TRANSACTION LEDGER BYZANTIUM VERSION 56c4578 - 2018-02-0229
 Value Mnemonic δ α
Description
0x40 BLOCKHASH
0x41 COINBASE
0x42 TIMESTAMP
0x43 NUMBER
0x44 DIFFICULTY
0x45 GASLIMIT
1 1
0 1 0 1 0 1 0 1 0 1
Get the hash of one of the 256 most recent complete blocks.
μ′s[0] ≡ P (IHp , μs[0], 0)
where P is the hash of a block of a particular number, up to a maximum age.
0 is left on the stack if the looked for block number is greater than the current block number or more than 256 blocks behind the current block.
0 
P(h,n,a)≡ h
P (Hp , n, a + 1)
if n>Hi∨a=256∨h=0 if n=Hi
otherwise
40s: Block Information
and we assert the header H can be determined as its hash is the parent hash in the block following it.
Get the block’s beneficiary address. μ′s[0] ≡ IHc
Get the block’s timestamp. μ′s[0] ≡ IHs
Get the block’s number. μ′s[0] ≡ IHi
Get the block’s difficulty. μ′s[0] ≡ IHd
Get the block’s gas limit. μ′s[0] ≡ IHl

 ETHEREUM: A SECURE DECENTRALISED GENERALISED TRANSACTION LEDGER BYZANTIUM VERSION 56c4578 - 2018-02-0230
 Value Mnemonic δ α
Description
Remove item from stack.
Load word from memory.
μ′s[0] ≡ μm[μs[0] . . . (μs[0] + 31)]
μ′i ≡max(μi,⌈(μs[0]+32)÷32⌉)
The addition in the calculation of μ′i is not subject to the 2256 modulo.
Save word to memory.
μ′m[μs[0] . . . (μs[0] + 31)] ≡ μs[1]
μ′i ≡max(μi,⌈(μs[0]+32)÷32⌉)
The addition in the calculation of μ′i is not subject to the 2256 modulo.
Save byte to memory.
μ′m[μs[0]] ≡ (μs[1] mod 256)
μ′i ≡max(μi,⌈(μs[0]+1)÷32⌉)
The addition in the calculation of μ′i is not subject to the 2256 modulo.
Load word from storage. μ′s[0] ≡ σ[Ia]s[μs[0]]
Save word to storage. σ′[Ia]s[μs[0]] ≡ μs[1]
0x50 POP
0x51 MLOAD
0x52 MSTORE
0x53 MSTORE8
0x54 SLOAD
0x55 SSTORE
0x56 JUMP
0x57 JUMPI
0x58 PC
0x59 MSIZE
0x5a GAS
0x5b JUMPDEST
1 0 1 1
2 0
2 0
1 1 2 0
1 0
2 0
0 1
0 1 0 1
0 0
 Gsset if
50s: Stack, Memory, Storage and Flow Operations
     CSSTORE (σ, μ) ≡
μs[1] ̸= 0 ∧ σ[Ia]s[μs[0]] = 0 Gsreset otherwise
A ′r ≡ A r +
 Rsclear 0
if μ [1] = 0 ∧ σ[Ia]s[μ [0]] ̸= 0 s s
otherwise
 Alter the program counter.
JJUMP(μ) ≡ μs[0]
This has the effect of writing said value to μpc. See section 9.
Conditionally alter the program counter.
  μs[0] if μs[1] ̸= 0 μpc + 1 otherwise
JJUMPI(μ) ≡
This has the effect of writing said value to μpc. See section 9.
 Get the value of the program counter prior to the increment corresponding to this instruction.
μ′s[0] ≡ μpc
Get the size of active memory in bytes. μ′s[0] ≡ 32μi
Get the amount of available gas, including the corresponding reduction for the cost of this instruction.
μ′s[0] ≡ μg
Mark a valid destination for jumps.
This operation has no effect on machine state during execution.

 ETHEREUM: A SECURE DECENTRALISED GENERALISED TRANSACTION LEDGER
BYZANTIUM VERSION 56c4578 - 2018-02-0231
 Value Mnemonic δ α
60s & 70s: Push Operations Description
Place 1 byte item on stack. μ′s[0] ≡ c(μpc + 1)
0x60 PUSH1
0x61 PUSH2
.
.....
0x7f PUSH32 0 1 Place 32-byte (full word) item on stack. μ′s[0] ≡ c (μpc + 1)...(μpc + 32)
where c is defined as above.
The bytes are right-aligned (takes the lowest significant place in big endian).
80s: Duplication Operations
0 1
0 1
where c(x) ≡
 Ib[x] if x < ∥Ib∥ 0 otherwise
The bytes are read in line from the program code’s bytes array.
The function c ensures the bytes default to zero if they extend past the limits. The byte is right-aligned (takes the lowest significant place in big endian).
Place 2-byte item on stack.
μ′s[0] ≡ c (μpc + 1)...(μpc + 2)
with c(x) ≡ (c(x0), ..., c(x∥x∥−1)) with c as defined as above.
The bytes are right-aligned (takes the lowest significant place in big endian).
  . . ..
   Value Mnemonic δ α 0x80 DUP112
Description
Duplicate 1st stack item. μ′s[0] ≡ μs[0]
 0x81 DUP223
. . .. .
Duplicate 2nd stack item. μ′s[0] ≡ μs[1]
 .....
0x8f DUP16 16 17 Duplicate 16th stack item.
μ′s[0] ≡ μs[15] 90s: Exchange Operations
   Value Mnemonic δ α
Description
Exchange 1st and 2nd stack items. μ′s[0] ≡ μs[1]
μ′s[1] ≡ μs[0]
Exchange 1st and 3rd stack items. μ′s[0] ≡ μs[2]
μ′s[2] ≡ μs[0]
0x90 SWAP1
0x91 SWAP2
2 2
3 3
  .... . .....
0x9f SWAP16 17 17 Exchange 1st and 17th stack items. μ′s[0] ≡ μs[16]
μ′s[16] ≡ μs[0]

 ETHEREUM: A SECURE DECENTRALISED GENERALISED TRANSACTION LEDGER BYZANTIUM VERSION 56c4578 - 2018-02-0232
 a0s: Logging Operations
For all logging operations, the state change is to append an additional log entry on to the substate’s log series: A lA′l ≡ Al · (Ia,t,μm[μs[0]...(μs[0] + μs[1] − 1)])
and to update the memory consumption counter: μ′i ≡M(μi,μs[0],μs[1])
The entry’s topic series, t, differs accordingly:
Value Mnemonic δ α 0xa0 LOG0 2 0
0xa1 LOG1 3 0 . . ..
Description
Append log record with no topics. t ≡ ()
Append log record with one topic. t ≡ (μs[2])
  . .....
 0xa4 LOG4 6 0 Append log record with four topics. t ≡ (μs[2],μs[3],μs[4],μs[5])

 ETHEREUM: A SECURE DECENTRALISED GENERALISED TRANSACTION LEDGER BYZANTIUM VERSION 56c4578 - 2018-02-0233
 Value Mnemonic δ α
Description
0xf0 CREATE
3 1
Create a new account with associated code. i ≡ μm[μs[1]...(μs[1] + μs[2] − 1)]
f0s: System operations
 Λ(σ∗,Ia,Io,L(μ ),Ip,μ [0],i,Ie +1,Iw) if μ [0] σ[Ia]b ∧ Ie <1024 ′′+gss
(σ ,μg,A ,o) ≡  σ,μg,∅  otherwise
σ∗ ≡ σ except σ∗[Ia]n = σ[Ia]n + 1
A′ ≡A A+ whichabbreviates: A′s ≡As ∪A+s ∧ A′l ≡Al ·A+l ∧ A′t ≡At ∪A+t ∧ A ′ r ≡ A r + A +r
μ′s[0] ≡ x
where x = 0 if the code execution for this operation failed due to an exceptional halting (orforaREVERT)σ′ =∅,orIe =1024
(the maximum call depth limit is reached) or μs[0] > σ[Ia]b (balance of the caller is too low to fulfil the value transfer); and otherwise x = A(Ia, σ[Ia]n), the address of the newly created account.
μ′i ≡M(μi,μs[1],μs[2])
μ′o ≡ ()
Thus the operand order is: value, input offset, input size.
 0xf1 CALL71
Message-call into an account.
i ≡ μm[μs[3]...(μs[3] + μs[4] − 1)]
 Θ(σ,Ia,Io,t,t,
′′+ s
(σ,g,A ,o)≡ CCALLGAS(μ),Ip,μs[2],μs[2],i,Ie +1,Iw) (σ, g, ∅, ())
if μ [2]   σ[Ia]b ∧ Ie <1024
otherwise
n ≡ min({μs[6],|o|})
μ′m[μs[5]...(μs[5] + n − 1)] = o[0...(n − 1)]
μ ′o = o
μ ′g ≡ μ g + g ′
μ′s[0] ≡ x
A′ ≡A A+
t≡μs[1] mod2160
where x = 0 if the code execution for this operation failed due to an exceptional halting (orforaREVERT)σ′ =∅orif
μs[2] > σ[Ia]b (not enough funds) or Ie = 1024 (call depth limit reached); x = 1 otherwise.
μ′i ≡M(M(μi,μs[3],μs[4]),μs[5],μs[6])
Thus the operand order is: gas, to, value, in offset, in size, out offset, out size.
CCALL (σ, μ) ≡ CGASCAP (σ, μ) + CEXTRA (σ, μ)
CCALLGAS (σ, μ) ≡
 CGASCAP(σ, μ) + Gcallstipend if μs[2] ̸= 0 CGASCAP (σ, μ) otherwise
CGASCAP (σ, μ) ≡
 min{L(μg − CEXTRA(σ, μ)), μs[0]} μs [0]
if μg ≥ CEXTRA(σ, μ) otherwise
CEXTRA (σ, μ) ≡ Gcall + CXFER (μ) + CNEW (σ, μ)
 Gnewaccount if DEAD(σ, μs[1] 0 otherwise
CXFER(μ) ≡ CNEW(σ,μ) ≡
mod 2160) ∧ μs[2] ̸= 0
 Gcallvalue if μs[2] ̸= 0 0 otherwise
 0xf2 CALLCODE 7 1
0xf3 RETURN 2 0
Message-call into this account with an alternative account’s code. Exactly equivalent to CALL except:
 Θ(σ∗,Ia,Io,Ia,t, if μ [2]   σ[Ia]b ∧ ′′+ s
(σ,g,A ,o)≡ CCALLGAS(μ),Ip,μs[2],μs[2],i,Ie +1,Iw) Ie <1024 (σ, g, ∅, ()) otherwise
Note the change in the fourth parameter to the call Θ from the 2nd stack value μs[1] (as in CALL) to the present address Ia. This means that the recipient is in fact the same account as at present, simply that the code is overwritten.
Halt execution returning output data.
HRETURN(μ) ≡ μm[μs[0] . . . (μs[0] + μs[1] − 1)]
This has the effect of halting the execution at this point with output defined. See section 9.
μ′i ≡M(μi,μs[0],μs[1])

 ETHEREUM: A SECURE DECENTRALISED GENERALISED TRANSACTION LEDGER BYZANTIUM VERSION 56c4578 - 2018-02-0234
 0xf4 DELEGATECALL 6 1
Message-call into this account with an alternative account’s code, but persisting
the current values for sender and value.
Compared with CALL, DELEGATECALL takes one fewer arguments. The omitted argument is μs[2]. As a result, μs[3], μs[4], μs[5] and μs[6] in the definition of CALL should respectively be replaced with μs[2], μs[3], μs[4] and μs[5].
Otherwise exactly equivalent to CALL except:  Θ(σ∗,Is,Io,Ia,t,
(σ′,g′,A+,o) ≡  μs[0],Ip,0,Iv,i,Ie + 1,Iw)
(σ, g, ∅, ()) otherwise
Note the changes (in addition to that of the fourth parameter) to the second and ninth parameters to the call Θ.
This means that the recipient is in fact the same account as at present, simply that the code is overwritten and the context is almost entirely identical.
Static message-call into an account.
Exactly equivalent to CALL except:
The argument μs[2] is replaced with 0.
The deeper argument μs[3], μs[4], μs[5] and μs[6] are respectively replaced with μs[2], μs[3], μs[4] and μs[5].
The last argument of Θ is ⊥.
Halt execution reverting state changes but returning data and remaining gas. The effect of this operation is described in (138).
For the gas calculation, we use the memory expansion function,
μ′i ≡M(μi,μs[0],μs[1])
Designated invalid instruction.
Halt execution and register account for later deletion.
0xfa STATICCALL 6 1
0xfd REVERT 2 0
0xfe INVALID ∅ ∅
0xff SELFDESTRUCT 1 0
if Iv  σ[Ia]b ∧ Ie <1024
    A ′s ≡ A s ∪ { I a } ∅
σ′[r] ≡ (σ[r]n, σ[r]b + σ[Ia]b, σ[r]s, σ[r]c) (σ[r]n, 0, σ[r]s, σ[r]c)
where r = μs[0] mod 2160 σ′[Ia]b = 0
n ≡ DEAD(σ, μs[0] mod 2160) ∧ σ[Ia]b ̸= 0
ifσ[r]=∅ ∧ σ[Ia]b =0 if r ̸= Ia
otherwise
 Rselfdestruct CSELFDESTRUCT(σ,μ) ≡ Gselfdestruct +
A ′r ≡ A r +
if Ia ∈/ As 0 otherwise
Appendix I. Genesis Block and is specified thus:
The genesis block is 15 items,
(306)   0256 , KEC RLP ()  , 0160 , stateRoot, 0, 0, 02048 , 217 , 0, 0, 3141592, time, 0, 0256 , KEC (42)  , (), ()
Where 0256 refers to the parent hash, a 256-bit hash which is all zeroes; 0160 refers to the beneficiary address, a 160-bit hash which is all zeroes; 02048 refers to the log bloom, 2048-bit of all zeros; 217 refers to the difficulty; the transaction trie root, receipt trie root, gas used, block number and extradata are both 0, being equivalent to the empty byte array. The sequences of both ommers and transactions are empty and represented by (). KEC (42)  refers to the Keccak hash of a byte array of length one whose first and only byte is of value 42, used for the nonce. KEC RLP ()   value refers to the hash of the ommer lists in RLP, both empty lists.
The proof-of-concept series include a development premine, making the state root hash some value stateRoot. Also time will be set to the initial timestamp of the genesis block. The latest documentation should be consulted for those values.
Appendix J. Ethash J.1. Definitions. We employ the following definitions:
 Gnewaccount if n
0 otherwise

 ETHEREUM: A SECURE DECENTRALISED GENERALISED TRANSACTION LEDGER
BYZANTIUM VERSION 56c4578 - 2018-02-0235
 Name Value
Jwordbytes 4 Jdatasetinit 230 Jdatasetgrowth 223 Jcacheinit 224 Jcachegrowth 217 Jepoch 30000 Jmixbytes 128 Jhashbytes 64 Jparents 256 Jcacherounds 3 Jaccesses 64
Description
Bytes in word.
Bytes in dataset at genesis.
Dataset growth per epoch.
Bytes in cache at genesis.
Cache growth per epoch.
Blocks per epoch.
mix length in bytes.
Hash length in bytes.
Number of parents of each dataset element. Number of rounds in cache production. Number of accesses in hashimoto loop.
  J.2. Size of dataset and cache. The size for Ethash’s cache c ∈ B and dataset d ∈ B depend on the epoch, which in turn depends on the block number.
  Hi   (307) Eepoch(Hi) = Jepoch
The size of the dataset growth by Jdatasetgrowth bytes, and the size of the cache by Jcachegrowth bytes, every epoch. In order to avoid regularity leading to cyclic behavior, the size must be a prime number. Therefore the size is reduced by a multiple of Jmixbytes, for the dataset, and Jhashbytes for the cache. Let dsize = ∥d∥ be the size of the dataset. Which is calculated using
(308) dsize = Eprime(Jdatasetinit + Jdatasetgrowth · Eepoch − Jmixbytes, Jmixbytes) The size of the cache, csize, is calculated using
(309) csize = Eprime(Jcacheinit + Jcachegrowth · Eepoch − Jhashbytes, Jhashbytes)
on the cache size csize and the seed hash s ∈ B32.
J.3.1. Seed hash. The seed hash is different for every epoch. For the first epoch it is the Keccak-256 hash of a series of
32 bytes of zeros. For every other epoch it is always the Keccak-256 hash of the previous seed hash: (311) s = Cseedhash(Hi)
  x if x/y∈P Eprime((x − 1) · y, y) otherwise
(310) Eprime(x, y) =
J.3. Dataset generation. In order to generate the dataset we need the cache c, which is an array of bytes. It depends
(312) Cseedhash(Hi) = With 032 being 32 bytes of zeros.
 KEC(032) if Eepoch(Hi) = 0 KEC(Cseedhash(Hi − Jepoch)) otherwise
J.3.2. Cache. The cache production process involves using the seed hash to first sequentially filling up csize bytes of memory, then performing Jcacherounds passes of the RandMemoHash algorithm created by Lerner [2014]. The initial cache c′, being an array of arrays of single bytes, will be constructed as follows.
We define the array ci, consisting of 64 single bytes, as the ith element of the initial cache:
 KEC512(s) if i = 0 KEC512(ci−1 ) otherwise
c′[i]=ci ∀ i<n
(316) c = Ecacherounds(c′, Jcacherounds) x

(317) Ecacherounds(x, y) = ERMH(x) Ecacherounds(ERMH(x), y − 1)
(313)
Therefore c′ can be defined as (314)
ci =
  csize   Jhashbytes
(315)
The cache is calculated by performing Jcacherounds rounds of the RandMemoHash algorithm to the inital cache c′:
n =
 Where a single round modifies each subset of the cache as follows:
(318) ERMH(x) =  Ermh(x, 0), Ermh(x, 1), ..., Ermh(x, n − 1)
if y=0 if y = 1 otherwise

 ETHEREUM: A SECURE DECENTRALISED GENERALISED TRANSACTION LEDGER BYZANTIUM VERSION 56c4578 - 2018-02-0236
(319) Ermh(x, i) = KEC512(x′[(i − 1 + n) mod n] ⊕ x′[x′[i][0] mod n])
with x′ = x except x′[j] = Ermh(x,j) ∀ j < i
J.3.3. Full dataset calculation. Essentially, we combine data from Jparents pseudorandomly selected cache nodes, and hash that to compute the dataset. The entire dataset is then generated by a number of items, each Jhashbytes bytes in size:
(320) d[i] = Edatasetitem(c, i) ∀ i <
In order to calculate the single item we use an algorithm inspired by the FNV hash (Glenn Fowler [1991]) in some cases
as a non-associative substitute for XOR.
(321) EFNV (x, y) = (x · (0x01000193 ⊕ y)) mod 232 The single item of the dataset can now be calculated as:
(322)
(323)
(324)
Eparents(c, i, p, m) =
 KEC512(c[i mod csize] ⊕ i)
Edatasetitem(c, i) = Eparents(c, i, −1, ∅)  Eparents(c,i,p+1,Emix(m,c,i,p+1))
if p<Jparents −2 otherwise
J.4. Proof-of-work function. Essentially, we maintain a “mix” Jmixbytes bytes wide, and repeatedly sequentially fetch Jmixbytes bytes from the full dataset and use the EFNV function to combine it with the mix. Jmixbytes bytes of sequential access are used so that each round of the algorithm always fetches a full page from RAM, minimizing translation lookaside buffer misses which ASICs would theoretically be able to avoid.
If the output of this algorithm is below the desired target, then the nonce is valid. Note that the extra application of KEC at the end ensures that there exists an intermediate nonce which can be provided to prove that at least a small amount of work was done; this quick outer PoW verification can be used for anti-DDoS purposes. It also serves to provide statistical assurance that the result is an unbiased, 256 bit number.
The PoW-function returns an array with the compressed mix as its first item and the Keccak-256 hash of the concatenation of the compressed mix with the seed hash as the second item:
(325)
PoW(Hn, Hn, d) = {mc(KEC(RLP(LH (Hn))), Hn, d), KEC(sh(KEC(RLP(LH (Hn))), Hn) + mc(KEC(RLP(LH (Hn))), Hn, d))}
Emix (m, c, i, p + 1)
if p = 0 Emix(m, c, i, p) = EFNV m, c[EFNV(i ⊕ p, m[p mod ⌊Jhashbytes/Jwordbytes⌋]) mod csize]  otherwise
  dsize   Jhashbytes
     With Hn being the hash of the header without the nonce. The compressed mix mc is obtained as follows: nmix
(326) mc(h, n, d) = Ecompress(Eaccesses(d,   sh(h, n), sh(h, n), −1), −4) i=0
The seed hash being:
(327) sh(h, n) = KEC512(h + Erevert(n)) Erevert(n) returns the reverted bytes sequence of the nonce n:
(328) Erevert(n)[i] = n[∥n∥ − i]
We note that the “+”-operator between two byte sequences results in the concatenation of both sequences. The dataset d is obtained as described in section J.3.3.
The number of replicated sequences in the mix is:
  Jmixbytes   (329) nmix = Jhashbytes
In order to add random dataset nodes to the mix, the Eaccesses function is used:
   Emixdataset (d, m, s, i)
(331) Emixdataset (d, m, s, i) = EFNV (m, Enewdata (d, m, s, i)
(330) Eaccesses (d, m, s, i) =
if i = Jaccesses − 2 Eaccesses (Emixdataset (d, m, s, i), s, i + 1) otherwise
Enewdata returns an array with nmix elements: (332)
Enewdata(d, m, s, i)[j] = d[EFNV(i ⊕ s[0], m[i The mix is compressed as follows:
mod
  Jmixbytes   Jwordbytes
])
mod
 dsize/Jhashbytes   nmix
· nmix + j] ∀
j < nmix
  (333)
Ecompress(m, i) =
 m
Ecompress(EFNV(EFNV(EFNV(m[i + 4], m[i + 5]), m[i + 6]), m[i + 7]), i + 8)
if i ∥m∥−8 otherwise

 ETHEREUM: A SECURE DECENTRALISED GENERALISED TRANSACTION LEDGER BYZANTIUM VERSION 56c4578 - 2018-02-0237
Appendix K. Anomalies on the Main Network
K.1. Deletion of an Account Despite Out-of-gas. At block 2675119, in the transaction 0xcf416c536ec1a19ed1fb89 e4ec7ffb3cf73aa413b3aa9b77d60e4fd81a4296ba, an account at address 0x03 was called and an out-of-gas occurred during the call. Against the equation (202), this added 0x03 in the set of touched addresses, and this transaction turned σ[0x03] into ∅.
Appendix L. List of mathematical symbols
 Symbol

Latex Command
\bigvee
Description
This is the least upper bound, supre- mum, or join of all elements operated on. Thus it is the greatest element of such elements (Davey and Priestley [2002]).
