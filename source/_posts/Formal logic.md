---
title: Formal logic
date: 2020-10-31 17:56:01
tags: Mathematics
category: Mathematics
---

The Dartmouth Conference in 1956 heralded the birth of artificial intelligence. In its infancy, the founding fathers, including future Turing Award winners such as John McCarthy, Herbert Simon, and Marvin Minsky, had a vision of how "a program capable of abstract thought could explain how synthetic matter could possess the mind of a human being".

In layman's terms, the ideal AI would be capable of learning, reasoning, and induction in the abstract, and would be far more versatile than the algorithms used to solve concrete problems like chess or Go.

An indispensable foundation for such an AI is formal logic. Early researchers of artificial intelligence believed that the basic unit of human cognition and thinking is symbols, and the cognitive process is the logical operation of symbols, so that human abstract logical thinking can be simulated by the operation of logic gates in the computer, and then realize the mechanization of human cognition.

In turn, formal logic is how intelligent behavior is described, and any system that can translate some physical patterns or symbols into other patterns or symbols has the potential to produce intelligent behavior, which is artificial intelligence.

Artificial intelligence is able to simulate intelligent behavior on the basis of having knowledge, but knowledge itself is also an abstract concept that needs to be represented in a way that a computer can understand.

In AI, common ways of representing knowledge include data structures and processing algorithms. Data structures are used to statically store the problem to be solved, the intermediate solution to the problem, the final solution to the problem, and the knowledge involved in the solution; processing algorithms are used to dynamically interact between existing problems and knowledge, and together they form a complete knowledge representation system.

In the study of artificial intelligence, it is a common method to realize knowledge representation by formal logic. Formal logic can be said to be all-encompassing, the simplest example of which is the three-stage theory proposed by the ancient Greek philosopher Aristotle and passed down to the present day, which consists of two premises and a conclusion.

- Science is constantly evolving.
- Artificial intelligence as science.
- So, artificial intelligence is constantly evolving.

Aristotle's contribution was not only to demonstrate the continuous development of artificial intelligence, but also to identify the formal process of deducing a conclusion on the basis of major and minor premises, a process that was completely free of content limitations. The resulting birth of symbolic reasoning has had a profound impact on the study of mathematical logic.

The main application in artificial intelligence is first-order predicate logic. Predicate logic is the most fundamental logical system and a fundamental part of formal logic. A special case of predicate logic is propositional logic. In predicate logic, a proposition is the basic unit of logical processing and can only be judged as true or false.

However, the limitation of propositional logic is that it cannot reflect the structure and logical features of the objects it describes, nor can it reflect the common features of different things. If the proposition "Old Li is Xiao Li's father" is given alone, the relationship between Old Li and Xiao Li cannot be determined without context, and the truth of the proposition is meaningless.

In order to extend the representational power of formal logic, predicate logic was born on the basis of propositional logic. Predicate logic splits propositions into individual words, predicates, and quantifiers with the following meanings.

- Individual words are concrete or abstract objects of description that can exist independently.
- Predicates are used to describe the properties and interrelationships of individual words.
- Quantifiers are used to describe the quantitative relationships of individual words, including the holomorphic quantifiers ∀ and the presence quantifiers.

All three elements can be used together to form a proposition. Different propositions can be linked by logical connectives to form compound propositions from simple propositions. There are five types of logical conjunctions, in descending order of priority.

- Negative (¬): compound proposition ¬P denies the truth value of proposition P, i.e. "not P".
- Hopefully (∧): Composite P∧Q denotes the sum of proposition P and Q, i.e. "P and Q".
- (∨): Complex P∨Q denotes the disjunction of proposition P or Q, i.e. "P or Q".
- Implicit (→): The compound proposition P→Q means that proposition P is a condition of proposition Q, i.e. "if P, then Q".
- Equivalence (↔): The compound proposition P↔Q means that proposition P and proposition Q are mutually implicit, i.e. "if P, then Q and if Q, then P".

Not only constant symbols appear in predicate logic, but also variable symbols are legal, and function symbols can also appear. The introduction of variables and functions expands the scope of predicate logic representation and enhances its universality. Predicate logic can be used to represent both factual knowledge such as concepts, states, and properties of things, as well as rule-based knowledge with definite causal relations between things.

Factual knowledge is usually represented by predicate formulas connected by parsimony and concatenation symbols, while rule-based knowledge is usually represented by predicate formulas connected by implication symbols. In a general sense, knowledge is expressed using predicate logic in the following steps.

- Define the predicate and the individual and determine the exact meaning of each predicate and each individual.
- Assign specific values to the variables in each predicate, depending on the thing or concept to be expressed.
- Connect the individual predicates with appropriate logical conjunctions based on the semantics of the knowledge to be expressed.

After the processing of the above steps, the abstract sense of knowledge can be transformed into a computer can recognize and process the data structure.

For example, if you want to use predicate logic to represent the knowledge that "all natural numbers are integers greater than zero", you first have to define all relations as corresponding predicates. The predicate N(x) means x is a natural number, P(x) means x is greater than zero, and I(x) means x is an integer, and then these predicates are connected semantically to obtain the predicate formula.
![](1.jpg)

The use of formal logic for knowledge representation is only a means to enable AI to automate reasoning, induction and deduction on the basis of knowledge in order to obtain new conclusions and new knowledge. At the present stage, the main difference between human intelligence and artificial intelligence is reflected in the reasoning ability.

Instead of plunging headlong into the vast amount of data to learn, human judgment is based on induction and deduction based on the characteristics of a small amount of data, with the general rules derived as the basis for judgment. A little interference in the digital image can make the neural network mistake a sea turtle for a rifle, but this trick cannot deceive human beings with thinking ability, and this is the reason why.

The basis for automatic reasoning by artificial intelligence is the generative system. Generative systems use generative rules to describe symbolic strings instead of operations to represent the process of reasoning and behavior as generative rules.

The generative rule is usually used to represent the causal relationship between things, and its basic form is P → Q. It can be used to represent the conclusion Q under the premise P, or the action Q under the condition P. Here P is called the rule antecedent, which can be a simple condition, or a complex condition formed by multiple simple conditions through a conjunction, while Q is called the rule postecedent.

When a set of generative rules cooperate and act in concert with each other, the conclusions generated by one generative rule can be used as known premises or conditions for another generative rule to further solve more complex problems, and such a system is a generative system.

Generative systems, in general, consist of three basic parts: the rule base, the fact base, and the reasoning machine.

The rule base is the core and foundation of the expert system, storing a collection of rules in the generative form, in which the completeness, accuracy and reasonableness of the rules will have a direct impact on the system performance.

The fact base stores input facts, intermediate results and final results. When the premise of a generating formula in the rule base can be matched with some known facts in the fact base, the generating formula is activated and its conclusion can be stored as a known fact in the fact base.

A reasoning machine, on the other hand, is a program used to control and coordinate the operation of the rule base with the fact base, and includes both a reasoning approach and a control strategy.

Specifically, there are three types of reasoning: forward reasoning, backward reasoning, and bidirectional reasoning.

Forward reasoning adopts a bottom-up approach, which starts from known facts and then derives the target conclusion by continuously selecting matching rule front pieces in the rule base to obtain matching rule back pieces.

Reverse reasoning uses a top-down approach, i.e., starting from the target hypothesis, one selects the matching rule antecedents by constantly matching the known facts with the rule antecedents in the rule base, and then backtracks to the known facts.

Two-way reasoning, on the other hand, is a more efficient way of combining forward and backward reasoning so that the reasoning proceeds in both top-down and bottom-up directions until it converges at some intermediate point.

Although automatic reasoning shows great ability in proving mathematical theorems, it is far from being intelligent in solving problems in daily life because of the lack of common sense. For humans, common sense is established through the process of socialization and growth.

However, computers are unable to reach understanding as humans grow up, so common sense, the prerequisite for intelligence, can only be instilled in a formalized manner on hard drives and in memory. This requires that the knowledge and beliefs of the average adult be explicitly expressed, flexibly organized and applied.

Without a clear expression of common sense and beliefs, AI is bound to get stuck in the mire of confusion, and obtaining generalized and adaptable intelligent behavior can only be a pipe dream.

The ultimate unavoidable and essential problem in talking about formal logic in AI lies in Gödel's incompleteness theorem.

In 1900, the German mathematician David Hilbert asked the 23 most important mathematical questions of the 20th century at the International Congress of Mathematicians in Paris, the second of which was the non-contradiction of arithmetic axiomatic systems.

In 1931, the Austrian mathematician Kurt Gödel gave a negative answer to this question, the First Incompleteness Theorem.

In any formal system containing elementary number theory.
All must have an undecidable proposition.
By this theorem, Gödel proves that the Achilles' heel of the axiomatized system is its inability to refer to itself, and that the following statement is a typical self-referential statement.

This mathematical proposition is not provable.
In the first place, this mathematical proposition discusses no other object than itself. The "present mathematical proposition" is a reference to the whole proposition. Secondly, the proposition gives a logical judgment that it cannot be proved.

Gödel proves that this proposition can neither be proven nor falsified, ruthlessly puncturing the dream that mathematical axiomatized systems have both consistency and completeness.

The impact of the incompleteness theorem on artificial intelligence lies in the understanding of the theoretical foundation that "the essence of cognition is computation". From the perspective of "cognition is computation", if a computer-based AI wants to achieve a thinking ability similar to that of human beings, it must also establish the concept of "self", which will undoubtedly lead to the emergence of self-referencing and become a living target of the incompleteness theorem.

If a computer can create a symbol to represent itself in its calculations, then Gödel's method of creating paradoxes can create illusions in the computer that are neither falsifiable nor verifiable.

In the shadow of Gödel's incompleteness theorem, the "cognitive computationalist" research agenda based on Turing's notion of computability has shown its great limitations. Today, the connectionist school, which relies on artificial neural networks, is flourishing, while the symbolist school, which relies on formal logic, is in decline.

But putting aside the differences in academic paths, what is the essential difference between human intelligence and artificial intelligence, and perhaps this is the greatest mystery left to us by the incompleteness theorem.

Today I share with you the essential foundations of formal logic necessary for artificial intelligence and the fundamentals of automatic reasoning using formal logic, the main points of which are as follows.

- If cognitive processes are defined as the logical manipulation of symbols, the basis of AI is formal logic.
(a) Predicate logic is the primary method of knowledge representation.
- Predicate logic-based systems allow for artificial intelligence with automatic reasoning capabilities.
- The incompleteness theorem challenges the fundamental notion of artificial intelligence that "cognition is essentially computational".

