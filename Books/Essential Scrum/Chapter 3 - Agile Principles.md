there is no such thing as a bad tool, rather just inappropriate times to use that tool.

- Plan-driven processes are so named because they attempt to plan for and anticipate up front all of the features a user might want in the end product, and to determine how best to build those features.
- The idea here is that the better the planning, the better the understanding, and therefore the better the execution.
- Plan-driven processes are often called sequential processes because practitioners perform, in sequence, a complete requirements analysis followed by a complete design followed in turn by coding/building and then testing.
- Plan-driven development works well if you are applying it to problems that are well defined, predictable, and unlikely to undergo any significant change.
- The problem, is not with the execution. It’s that plan-driven approaches are based on a set of beliefs that do not match the uncertainty inherent in most product development efforts.
- **Scrum, on the other hand, is based on a different set of beliefs—ones that do map well to problems with enough uncertainty to make high levels of predictability difficult.**
- ![[Pasted image 20250916134908.png]]

### Variability and Uncertainty
Scrum leverages the **[variability](https://learning-oreilly-com.proxy.library.nyu.edu/library/view/essential-scrum-a/9780321700407/gloss01.html#gloss01_220)** and **[uncertainty](https://learning-oreilly-com.proxy.library.nyu.edu/library/view/essential-scrum-a/9780321700407/gloss01.html#gloss01_212)** in product development to create innovative solutions. I describe four principles related to this topic:

• [Embrace helpful variability](https://learning-oreilly-com.proxy.library.nyu.edu/library/view/essential-scrum-a/9780321700407/ch03.html#ch03lev2sec1).
• [Employ iterative and incremental development](https://learning-oreilly-com.proxy.library.nyu.edu/library/view/essential-scrum-a/9780321700407/ch03.html#ch03lev2sec2).
• [Leverage variability through inspection, adaptation, and transparency](https://learning-oreilly-com.proxy.library.nyu.edu/library/view/essential-scrum-a/9780321700407/ch03.html#ch03lev2sec3).
• [Reduce all forms of uncertainty simultaneously](https://learning-oreilly-com.proxy.library.nyu.edu/library/view/essential-scrum-a/9780321700407/ch03.html#ch03lev2sec4).

#### Embrace Helpful Variability
![[Pasted image 20250916135232.png]]
- In product development, the goal is to create the unique _single instance_ of the product, not to _manufacture_ the product.
- This single instance is analogous to a unique recipe.
- we want to create a unique recipe for a new product. Some amount of variability is necessary to produce a different product each time. 
- In fact, every feature we build within a product is different from every other feature within that product, so we need variability even at this level.
- Only once we have the recipe do we manufacture the product—in the case of software products, as easily as copying bits.

#### Employ Iterative and Incremental Development
- **[Iterative development](https://learning-oreilly-com.proxy.library.nyu.edu/library/view/essential-scrum-a/9780321700407/gloss01.html#gloss01_097)** acknowledges that we will probably get things wrong before we get them right and that we will do things poorly before we do them well ([Goldberg and Rubin 1995](https://learning-oreilly-com.proxy.library.nyu.edu/library/view/essential-scrum-a/9780321700407/bib01.html#bib01_019)). 
- As such, iterative development is a planned rework strategy.
- We use multiple passes to improve what we are building so that we can converge on a good solution.
- The biggest downside to iterative development is that in the presence of uncertainty it can be difficult up front to determine (plan) how many improvement passes will be necessary.
- we break the product into smaller pieces so that we can build some of it, learn how each piece is to survive in the environment in which it must exist, adapt based on what we learn, and then build more of it.
- Incremental development gives us important information that allows us to adapt our development effort and to change how we proceed. The biggest drawback to incremental development is that by building in pieces, we risk missing the big picture (we see the trees but not the forest).
- Scrum leverages the benefits of both iterative and incremental development, while negating the disadvantages of using them individually.
- Scrum does this by using both ideas in an adaptive series of timeboxed iterations called sprints (see Figure)
- ![[Pasted image 20250916140544.png]]
- During each sprint we perform all of the activities necessary to create a working product increment (some of the product, not all of it).
- This is illustrated in [Figure 3.4](https://learning-oreilly-com.proxy.library.nyu.edu/library/view/essential-scrum-a/9780321700407/ch03.html#ch03fig04) by showing that some analysis, design, build, integration, and test work is completed in each sprint.
- This all-at-once approach has the benefit of quickly validating the assumptions that are made when developing product features.
- For example, we make some design decisions, create some code based on those decisions, and then test the design and code—all in the same sprint.
- By doing all of the related work within one sprint, we are able to quickly rework features, thus achieving the benefits of iterative development, without having to specifically plan for additional iterations.
- A misuse of the sprint concept is to focus each sprint on just one type of work—for example, sprint 1 (analysis), sprint 2 (design), sprint 3 (coding), and sprint 4 (testing). Such an approach attempts to overlay Scrum with a waterfall-style work breakdown structure.
- **In Scrum, we don’t work on a phase at a time; we work on a feature at a time.**
- **That increment includes or is integrated and tested with any previously developed features; otherwise, it is not considered done.**
- For example, increment 2 in [Figure 3.4](https://learning-oreilly-com.proxy.library.nyu.edu/library/view/essential-scrum-a/9780321700407/ch03.html#ch03fig04) includes the features of increment 1. At the end of the sprint, we can get feedback on the newly completed features within the context of already completed features. This helps us view the product from more of a big-picture perspective than we might otherwise have.
- We can choose different features to work on in the next sprint or alter the process we will use to build the next set of features.
- This helps overcome the issue of not knowing up front exactly how many improvement passes we will need.
- **Scrum does not require that we predetermine a set number of iterations. The continuous stream of feedback will guide us to do the appropriate and economically sensible number of iterations while developing the product incrementally.**

#### Leverage Variability through Inspection, Adaptation, and Transparency
- ![[Pasted image 20250916143226.png]]
- **Scrum embraces the fact that in product development, some level of variability is required in order to build something new.**
- **Scrum also assumes that the process necessary to create the product is complex and therefore would defy a complete up-front definition.**
- At the heart of Scrum are the principles of **[inspection](https://learning-oreilly-com.proxy.library.nyu.edu/library/view/essential-scrum-a/9780321700407/gloss01.html#gloss01_090)**, **[adaptation](https://learning-oreilly-com.proxy.library.nyu.edu/library/view/essential-scrum-a/9780321700407/gloss01.html#gloss01_006)**, and **[transparency](https://learning-oreilly-com.proxy.library.nyu.edu/library/view/essential-scrum-a/9780321700407/gloss01.html#gloss01_209)** (referred to collectively by [Schwaber and Beedle 2001](https://learning-oreilly-com.proxy.library.nyu.edu/library/view/essential-scrum-a/9780321700407/bib01.html#bib01_048) as **[empirical process control](https://learning-oreilly-com.proxy.library.nyu.edu/library/view/essential-scrum-a/9780321700407/gloss01.html#gloss01_053)**). In Scrum, we inspect and adapt not only what we are building but also how we are building it (see [Figure 3.5](https://learning-oreilly-com.proxy.library.nyu.edu/library/view/essential-scrum-a/9780321700407/ch03.html#ch03fig05)).
- ![[Pasted image 20250916143702.png]]
- To do this well, we rely on transparency: all of the information that is important to producing a product must be available to the people involved in creating the product.
- Transparency makes inspection possible, which is needed for adaptation.
- **Transparency also allows everyone concerned to observe and understand what is happening. It leads to more communication and it establishes trust (both in the process and among team members).**

#### Reduce All Forms of Uncertainty Simultaneously
- Developing new products is a complex endeavor with a high degree of uncertainty. That uncertainty can be divided into two broad categories ([Laufer 1996](https://learning-oreilly-com.proxy.library.nyu.edu/library/view/essential-scrum-a/9780321700407/bib01.html#bib01_029)):

	- **[End uncertainty](https://learning-oreilly-com.proxy.library.nyu.edu/library/view/essential-scrum-a/9780321700407/gloss01.html#gloss01_054)** (_what_ uncertainty)—uncertainty surrounding the features of the final product
	- **[Means uncertainty](https://learning-oreilly-com.proxy.library.nyu.edu/library/view/essential-scrum-a/9780321700407/gloss01.html#gloss01_105)** (_how_ uncertainty)—uncertainty surrounding the process and technologies used to develop a product.
	- In particular environments or with particular products there might also be **[customer uncertainty](https://learning-oreilly-com.proxy.library.nyu.edu/library/view/essential-scrum-a/9780321700407/gloss01.html#gloss01_037)** (_who_ uncertainty).

Traditional, sequential development processes focus first on eliminating all end uncertainty by fully defining up front what is to be built, and only then addressing means uncertainty.

This simplistic, linear approach to uncertainty reduction is ill suited to the complex domain of product development, where our actions and the environment in which we operate mutually constrain one another. For example:

• We decide to build a feature (our action).

• We then show that feature to a customer, who, once he sees it, changes his mind about what he really wants, or realizes that he did not adequately convey the details of the feature (our action elicits a response from the environment).

• We make design changes based on the feedback (the environment’s reaction influences us to take another unforeseen action).

- In Scrum, we do not constrain ourselves by fully addressing one type of uncertainty before we address the next type.
- Instead, we take a more holistic approach and focus on simultaneously reducing all uncertainties (end, means, customer, and so on).
- Simultaneously addressing multiple types of uncertainty is facilitated by iterative and incremental development and guided by constant inspection, adaptation, and transparency.
- Such an approach allows us to opportunistically probe and explore our environment to identify and learn about the **[unknown unknowns](https://learning-oreilly-com.proxy.library.nyu.edu/library/view/essential-scrum-a/9780321700407/gloss01.html#gloss01_213)** (the things that we don’t yet know that we don’t know) as they emerge.
### Prediction and Adaptation
When using Scrum, we are constantly balancing the desire for prediction with the need for adaptation. I describe five agile principles related to this topic:
• [Keep options open](https://learning-oreilly-com.proxy.library.nyu.edu/library/view/essential-scrum-a/9780321700407/ch03.html#ch03lev2sec5).
• [Accept that you can’t get it right up front](https://learning-oreilly-com.proxy.library.nyu.edu/library/view/essential-scrum-a/9780321700407/ch03.html#ch03lev2sec6).
• [Favor an adaptive, exploratory approach](https://learning-oreilly-com.proxy.library.nyu.edu/library/view/essential-scrum-a/9780321700407/ch03.html#ch03lev2sec7).
• [Embrace change in an economically sensible way](https://learning-oreilly-com.proxy.library.nyu.edu/library/view/essential-scrum-a/9780321700407/ch03.html#ch03lev2sec8).
• [Balance predictive up-front work with adaptive just-in-time work](https://learning-oreilly-com.proxy.library.nyu.edu/library/view/essential-scrum-a/9780321700407/ch03.html#ch03lev2sec9).

#### Keep Options Open
- Plan-driven, sequential development requires that important decisions in areas like requirements or design be made, reviewed, and approved within their respective phases. 
- Furthermore, these decisions must be made before we can transition to the next phase, even if those decisions are based on limited knowledge.
- **Scrum contends that we should never make a premature decision just because a generic process would dictate that now is the appointed time to make one.**
- Instead, when using Scrum, we favor a strategy of keeping our options open. Often this principle is referred to as the **[last responsible moment](https://learning-oreilly-com.proxy.library.nyu.edu/library/view/essential-scrum-a/9780321700407/gloss01.html#gloss01_101)** (**[LRM](https://learning-oreilly-com.proxy.library.nyu.edu/library/view/essential-scrum-a/9780321700407/gloss01.html#gloss01_104)**) ([Poppendieck and Poppendieck 2003](https://learning-oreilly-com.proxy.library.nyu.edu/library/view/essential-scrum-a/9780321700407/bib01.html#bib01_040)), meaning that we delay commitment and do not make important and irreversible decisions until the last responsible moment.
- And when is that? When the cost of not making a decision becomes greater than the cost of making a decision (see [Figure 3.6](https://learning-oreilly-com.proxy.library.nyu.edu/library/view/essential-scrum-a/9780321700407/ch03.html#ch03fig06)). At that moment, we make the decision.
- ![[Pasted image 20250916175126.png]]
- On the first day of a product development effort we have the least information about what we are doing. On each subsequent day of the development effort, we learn a little more. Why, then, would we ever choose to make all of the most critical, and perhaps irreversible, decisions on the first day or very early on?
- As we acquire a better understanding regarding the decision, the cost of deciding declines (the likelihood of making a bad decision declines because of increasing market or technical certainty). That’s why we should wait until we have better information before committing to a decision.

#### Accept That You Can’t Get It Right Up Front
- In Scrum, we acknowledge that we can’t get all of the requirements or the plans right up front. In fact, we believe that trying to do so could be dangerous because we are likely missing important knowledge, leading to the creation of a large quantity of low-quality requirements
- ![[Pasted image 20250916182412.png]]
- This figure illustrates that when using a plan-driven, sequential process, a large number of requirements are produced early on when we have the least cumulative knowledge about the product. This approach is risky, because there is an illusion that we have eliminated end uncertainty. It is also potentially very wasteful when our understanding improves or things change
- With Scrum, we still produce some requirements and plans up front, but just sufficiently, and with the assumption that we will fill in the details of those requirements and plans as we learn more about the product we are building.
#### Favor an Adaptive, Exploratory Approach

![[Pasted image 20250916184034.png]]

- Scrum favors a adaptive, trial-and-error approach based on appropriate use of exploration.
- **[Exploration](https://learning-oreilly-com.proxy.library.nyu.edu/library/view/essential-scrum-a/9780321700407/gloss01.html#gloss01_061)** refers to times when we choose to gain knowledge by doing some activity, such as building a prototype, creating a proof of concept, performing a study, or conducting an experiment. In other words, when faced with uncertainty, we buy information by exploring.
- In Scrum, if we have enough knowledge to make an informed, reasonable step forward with our solution, we advance. 
- However, when faced with uncertainty, rather than trying to predict it away, we use low-cost exploration to buy relevant information that we can then use to make an informed, reasonable step forward with our solution.
- The feedback from our action will help us determine if and when we need further exploration.

#### Embrace Change in an Economically Sensible Way
![[Pasted image 20250916184042.png]]
Unfortunately, being excessively predictive in early-activity phases often has the opposite effect. It not only fails to eliminate change; it actually contributes to deliveries that are late and over budget as well. Why this paradoxical truth?
- First, the desire to eliminate expensive change forces us to overinvest in each phase—doing more work than is necessary and practical.
- Second, we’re forced to make decisions based on important assumptions early in the process, before we have validated these assumptions with feedback from our stakeholders based on our working assets.
- As a result, we produce a large inventory of work products based on these assumptions. Later, this inventory will likely have to be corrected or discarded as we validate (or invalidate) our assumptions, or change happens (for example, requirements emerge or evolve), as it always will. This fits the classic pattern of a self-fulfilling prophecy
- ![[Pasted image 20250916184508.png]]
- In Scrum, we assume that change is the norm. We believe that we can’t predict away the inherent uncertainty that exists during product development by working longer and harder up front. Thus, we must be prepared to embrace change.
- And when that change occurs, we want the economics to be more appealing than with traditional development, even when the change happens later in the product development effort.
- Our goal, therefore, is to keep the cost-of-change curve flat for as long as possible—making it economically sensible to embrace even late change. [Figure 3.11](https://learning-oreilly-com.proxy.library.nyu.edu/library/view/essential-scrum-a/9780321700407/ch03.html#ch03fig11) illustrates this idea.
- ![[Pasted image 20250916184639.png]]
- by carefully controlling how much work is in progress and how it flows through the Scrum process, changes (like new requirements) stay relatively affordable no matter when they occur, unlike in sequential models where late changes are very expensive.
- Regardless of which product development approach we use, we want the following relationship to be true: a small change in requirements should yield a proportionally small change in implementation and therefore in cost
- Another desirable property of this relationship is that we want it to be true regardless of _when_ the change request is made.
- With Scrum, we produce many work products (such as detailed requirements, designs, and test cases) in a just-in-time fashion, avoiding the creation of potentially unnecessary artifacts.
- 