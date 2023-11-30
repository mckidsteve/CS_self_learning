## Get the code获取代码

[](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@get_started)To get started,要开始，

1. [](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@ask_didit_create)Ask Didit to 要求Didit[create a remote `psets/ps1` repository](https://didit.mit.edu/6.031/sp21/psets/ps1)[创建远程`psets/ps1`存储库](https://didit.mit.edu/6.031/sp21/psets/ps1) for you on github.mit.edu.给你github.mit.edu。
    
2. [](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@clone_repo_find)Clone the repo. Find the 克隆存储库。 找到`git clone` command at the top of your Didit assignment page, copy it entirely, paste it into your terminal, and run it.Didit分配页面顶部的命令，将其完全复制，将其粘贴到您的终端中，然后运行它。
    
3. [](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@import_into_eclipse)Import into Eclipse. See 导入Eclipse。 看[Problem Set 0](http://web.mit.edu/6.031/www/sp21/psets/ps0/#clone)[问题集0](http://web.mit.edu/6.031/www/sp21/psets/ps0/#clone) for if you need a refresher on how to create, clone, or import your repository.如果您需要复习如何创建、克隆或导入存储库。
    

## [](http://web.mit.edu/6.031/www/sp21/psets/ps1/#overview)Overview概览

[](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@you_ever_used)Have you ever used 你有没有用过[flashcards](https://en.wikipedia.org/wiki/Flashcard)[抽认卡](https://en.wikipedia.org/wiki/Flashcard) – maybe to improve your vocabulary either in your native language or in a foreign language you want to learn? A flashcard has two sides, a front and a back, which might be a vocabulary word and its meaning. To use a flashcard for learning practice, you look at the front and try to recall what’s on the back, then look at the back to see if you were right. Flashcards are an example of \-也许可以用你的母语或你想学的外语来改进你的词表？ 抽认卡有正反面，这可能是一个词表及其含义。 要使用抽认卡进行学习练习，你看前面，试着召回背面的内容，然后看背面，看看你是否正确。 抽认卡是一个例子[active learning](https://en.wikipedia.org/wiki/Active_learning)[主动学习](https://en.wikipedia.org/wiki/Active_learning), which is much more effective than passive reading or listening.，这比被动阅读或倾听要有效得多。

SFBSFB safe from bugs防虫

ETUETU支持 easy to under­stand容易理解

RFCRFC ready for change准备改变

[](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@flashcard_system_scheduling)A A_flashcard system__抽认卡系统_ is a scheduling algorithm that decides when to practice each flashcard for the most efficient learning. Flashcard systems take advantage of 是一种调度算法，它决定何时练习每个抽认卡以获得最有效的学习。 抽认卡系统利用[spaced repetition](https://en.wikipedia.org/wiki/Spaced_repetition)[间隔重复](https://en.wikipedia.org/wiki/Spaced_repetition), the principle that we learn more durably when practice is spaced out over time rather than crammed together, and furthermore that the ，这一原则是，当实践随着时间的推移被隔开而不是挤在一起时，我们会学得更持久，此外[rate of forgetting something](https://en.wikipedia.org/wiki/Forgetting_curve)[忘记某事的速度](https://en.wikipedia.org/wiki/Forgetting_curve) is slower the more we have practiced it, so that the spacing between practice can increase as a concept or skill becomes more learned.我们练习得越多，就越慢，这样练习之间的行间距就会随着一个概念或技能变得越来越学成而增加。

[](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@problem_set_uses)This problem set uses a spaced repetition flashcard algorithm that we will call the 这个问题集使用间隔重复抽认卡算法，我们称之为_Modified-Leitner__改性-莱特纳_ system, because it is inspired by the 系统，因为它的灵感来自[Leitner system](https://en.wikipedia.org/wiki/Leitner_system)[莱特纳系统](https://en.wikipedia.org/wiki/Leitner_system) but differs in the details. In a Leitner system, flashcards are grouped into numbered 但细节不同。 在Leitner系统中，抽认卡被分组为编号_learning buckets__学习桶_. Low-numbered buckets contain flashcards that are less well-learned, so low-numbered buckets are scheduled for practice more often than high-numbered buckets. As a flashcard receives more and more successful practice, it moves up to higher buckets, until finally reaching a . 低编号的桶包含较少学成的抽认卡，因此低编号的桶比高编号的桶更经常被安排练习。 随着抽认卡获得越来越多的成功实践，它会上升到更高的桶，直到最终达到_retired__退休_ bucket (e.g., bucket 5). Flashcards in the retired bucket are considered well-enough learned not to need practice anymore.桶（例如，桶5）。 退休桶里的抽认卡被认为是足够好的学成，不再需要练习了。

[](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@here_specific_behavior)Here is the specific behavior of our Modified-Leitner algorithm:以下是我们修改后的Leitner算法的具体行为：

- [](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@new_card_before)A new card, before its first practice, is assumed to be in bucket 0.一张新卡，在第一次练习之前，假设在桶0中。
- On day 在白天_n__n_ of the learning process, the user collects all the buckets that are scheduled for practice that day, and practices every card found in those buckets. 在学习过程中，用户收集当天安排练习的所有桶，并练习在这些桶中找到的每张卡片。
    - Cards in the retired bucket are not scheduled for practice.
    - 退役桶中的卡片不安排练习。
    - Bucket 0 cards are practiced every day, bucket 1 cards every two days, bucket 2 cards every four days, and so forth. In general, bucket _i_ is practiced every 2<sup mt_node="origin_leaf"><em mt_node="origin_leaf">i</em></sup> days.
    - 桶0卡每天练习，桶1卡每两天练习，桶2卡每四天练习，依此类推。一般来说，桶i每2i天练习一次。
    - The user is of course free to practice new cards, or unscheduled cards of their own choice, or to practice cards repeatedly.
    - 用户当然可以自由练习新卡，或者自己选择的非预定卡，或者重复练习卡。
- Whenever the user practices a flashcard: 每当用户练习抽认卡时：
    - if they get the card **wrong** – they are unable to correctly recall what’s on the back of the card – then the card is put back in bucket 0.
    - 如果他们把卡片弄错了——他们无法正确召回卡片背面的东西——然后卡片被放回桶0。
    - if they find the card **easy** – getting it right without much mental effort – then the card moves up from bucket _i_ to bucket _i+1_ (unless the card is already in the retired bucket).
    - 如果他们发现卡片很容易——不用太多脑力就能把它弄好——那么卡片就会从桶i向上移动到桶i+1（除非卡片已经在退役的桶中）。
    - if they find the card **hard** – getting it right but only with effort – then the card moves down to bucket _i-1_ (unless the card is already in bucket 0).
    - 如果他们发现这张卡很难——但只有努力才能正确——那么这张卡就会向下移动到桶i-1（除非这张卡已经在桶0中）。

[](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@problem_set_consists)This problem set consists of a group of methods that can be used to implement the Modified-Leitner flashcard system:这个问题集由一组方法组成，这些方法可用于实现Modified-Leitner抽认卡系统：

- [](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@tobucketsets_converts_map)[`toBucketSets()`](http://web.mit.edu/6.031/www/sp21/psets/ps1/doc/flashcards/BucketSets.html)[`toBucketSets()`](http://web.mit.edu/6.031/www/sp21/psets/ps1/doc/flashcards/BucketSets.html) converts a 转换`Map` representation of learning buckets into a 将学习桶表示为`List`\-of-\-of-`Set` representation 表象
- [`getBucketRange()`](http://web.mit.edu/6.031/www/sp21/psets/ps1/doc/flashcards/BucketRange.html) finds the range of buckets that contain flashcards, as a rough measure of progress
- [`getBucketRange()`](http://web.mit.edu/6.031/www/sp21/psets/ps1/doc/flashcards/BucketRange.html)查找包含抽认卡的存储桶范围，作为进度的粗略度量
- [`practice()`](http://web.mit.edu/6.031/www/sp21/psets/ps1/doc/flashcards/Practice.html) selects cards to practice on a particular day
- [`practice()`](http://web.mit.edu/6.031/www/sp21/psets/ps1/doc/flashcards/Practice.html)选择卡片在特定的一天练习
- [`update()`](http://web.mit.edu/6.031/www/sp21/psets/ps1/doc/flashcards/Update.html) updates a card’s bucket number after a practice trial
- [`update()`](http://web.mit.edu/6.031/www/sp21/psets/ps1/doc/flashcards/Update.html)在练习后更新卡的存储桶号
- [`deduplicate()`](http://web.mit.edu/6.031/www/sp21/psets/ps1/doc/flashcards/Deduplicate.html) consolidates a set of flashcards by finding redundant cards
- [`deduplicate()`](http://web.mit.edu/6.031/www/sp21/psets/ps1/doc/flashcards/Deduplicate.html)通过查找冗余卡片来整合一组抽认卡

[](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@addition_implementing_these)In addition to implementing these methods yourself, you will also write a test suite for each method, carefully following the method’s spec. Your test suites will be graded in part by running them against various staff implementations of the method, some of which obey the spec and some which don’t. A good test suite should pass the good implementations and reject the bad ones.除了自己实现这些方法之外，您还将为每个方法编写一个测试套件，仔细遵循该方法的规范。 您的测试套件将部分通过根据该方法的各种人员实现运行它们来评分，其中一些符合规范，一些不符合规范。 一个好的测试套件应该通过好的实现，拒绝坏的实现。

[](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@you_also_asked)You are also asked to change exactly one of these method specs, 您还被要求更改其中一个方法规范，`deduplicate()`. Its current spec is very weak, so you will strengthen the spec, and test and implement your stronger spec. Unlike your test suites for other methods, your . 它目前的规范非常弱，所以你将加强规范，并测试和实施你更强的规范。 与其他方法的测试套件不同，您的`deduplicate()` tests won’t be run against good and bad staff implementations, because you’ll be changing the spec.测试不会针对好的和坏的员工实现运行，因为您将更改规范。

## [](http://web.mit.edu/6.031/www/sp21/psets/ps1/#steps)Steps步骤

[](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@since_doing_iterative)Since we are doing 既然我们正在做[iterative test-first programming](http://web.mit.edu/6.031/www/sp21/classes/03-testing/#iterative_test-first_programming)[迭代测试优先编程](http://web.mit.edu/6.031/www/sp21/classes/03-testing/#iterative_test-first_programming), your workflow for each method should follow the order specs → tests → implementation, iterating as needed. Make steady progress across methods and down the workflow, in the following pattern:，每个方法的工作流程应遵循顺序规范→测试→实施，根据需要进行迭代。 按照以下模式在方法和工作流程中取得稳步进展：

1. [](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@read_entire_handout)Read this entire handout first.首先阅读整个讲义。
2. Read the specs for all methods carefully.
3. 仔细阅读所有方法的规范。
4. For 为`toBucketSets()` and 和`getBucketRange()`: :
    1. Write tests for one method. Add/commit/push.
    2. 为一种方法编写测试。添加/提交/推送。
    3. Write tests for the other method. Add/commit/push.
    4. 为其他方法编写测试。添加/提交/推送。
    5. Implement one method. Add/commit/push.
    6. 实现一种方法。添加/提交/推送。
    7. Implement the other method. Add/commit/push.
    8. 实现另一种方法。添加/提交/推送。
5. For 为`practice()` and 和`update()`: :
    1. Write tests for one method. Add/commit/push.
    2. 为一种方法编写测试。添加/提交/推送。
    3. Write tests for the other method. Add/commit/push.
    4. 为其他方法编写测试。添加/提交/推送。
    5. Implement one method. Add/commit/push.
    6. 实现一种方法。添加/提交/推送。
    7. Implement the other method. Add/commit/push.
    8. 实现另一种方法。添加/提交/推送。
6. For 为`deduplicate()`: :
    1. Write tests for the staff-provided weak spec. Add/commit/push.
    2. 为员工提供的弱规范编写测试。添加/提交/推送。
    3. Code a trivial implementation of the weak spec, to exercise your tests. Add/commit/push.
    4. 编写一个弱规范的简单实现，以练习您的测试。添加/提交/推送。
    5. Strengthen the spec, following instructions later in this handout. Add/commit/push.
    6. 按照本讲义后面的说明加强规范。添加/提交/推送。
    7. Add tests for the revised spec. Add/commit/push.
    8. 为修订后的规范添加测试。添加/提交/推送。
    9. Implement the revised spec. Add/commit/push.
    10. 实施修订后的规范。添加/提交/推送。
    11. Iterate as needed. Add/commit/push after each step.
    12. 根据需要迭代。在每个步骤后添加/提交/推送。

|  |  | [](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@tobucketsets_getbucketrange_practice)
`toBucketSets``toBucketSets`

 | 

`getBucketRange``getBucketRange`

 | 

`practice``practice`

 | 

`update``update`

 | 

`deduplicate``deduplicate`

 | 

 ...iterate...…迭代…

 | 

 ...iterate...…迭代…

 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| spec | 1 | 1 | 1 | 1 | 1 | 4c | 4f |
| test | 2a | 2b | 3a | 3b | 4a | 4d | 4f |
| implement | 2c | 2d | 3c | 3d | 4b | 4e | 4f |
| 规格 | 1 | 1 | 1 | 1 | 1 | 4c | 4f |
| 测试 | 2a | 2b | 3a | 3b | 4a | 4d | 4f |
| 实现 | 2c | 2d | 3c | 3d | 4b | 4e | 4f |

[](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@next_few_sections)The next few sections of this handout have more specific advice about these steps:本讲义接下来的几节对这些步骤有更具体的建议：

- [](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@specifications_testing_implementation)[Specifications](http://web.mit.edu/6.031/www/sp21/psets/ps1/#specifications)[规格](http://web.mit.edu/6.031/www/sp21/psets/ps1/#specifications)
- [Testing](http://web.mit.edu/6.031/www/sp21/psets/ps1/#testing)
- [测试](http://web.mit.edu/6.031/www/sp21/psets/ps1/#testing)
- [Implementation](http://web.mit.edu/6.031/www/sp21/psets/ps1/#implementation)
- [实施](http://web.mit.edu/6.031/www/sp21/psets/ps1/#implementation)
- [Strengthening `deduplicate()`](http://web.mit.edu/6.031/www/sp21/psets/ps1/#strengthening_deduplicate)
- [加强`deduplicate()`](http://web.mit.edu/6.031/www/sp21/psets/ps1/#strengthening_deduplicate)

[](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@use_git_add-git)Use 使用`git add`//`git commit`//`git push` after every step that changes your code. Committing frequently – whenever you’ve written some tests, fixed a bug, or added a new feature – is a good way to use version control, and will be a good habit to have for your team projects and as a software engineer in general, so start forming the habit now. Your git commit history for this problem set should demonstrate test-first programming, have frequent small commits, and include short but descriptive commit messages.在更改代码的每一步之后。 经常提交——每当你编写了一些测试、修复了一个bug或添加了一个新特征——是使用版本控制的好方法，并且对于你的团队项目和作为一个软件工程师来说都是一个好习惯，所以现在就开始养成这个习惯。 这个问题集的git提交历史应该演示测试优先编程，有频繁的小提交，并包括简短但描述性的提交消息。

## [](http://web.mit.edu/6.031/www/sp21/psets/ps1/#what_you_can_and_cant_change)What you can and can’t change你能改变什么不能改变什么

[](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@order_your_overall)In order for your overall program to meet the specification of this problem set, you are required to keep some things unchanged:为了让您的整个程序满足此问题集的规范，您需要保持一些不变：

- [](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@don-t_change_these)**Don’t change these classes at all:****根本不要更改这些类：** the classes 班级`Flashcard` and 和`AnswerDifficulty` should not be modified 不应修改_at all__完全没有_..
- **Don’t change these class names:** the classes `BucketSets`, `BucketRange`, `Practice`, `Update`, `Deduplicate`, `BucketSetsTest`, `BucketRangeTest`, `PracticeTest`, `UpdateTest`, and `DeduplicateTest` must use those names and remain in the `flashcards` package.
- **不要更改这些类名：**这些类`BucketSets`、`BucketRange`、`Practice`、`Update`、`Deduplicate`、`BucketSetsTest`、`BucketRangeTest`、`PracticeTest`、`UpdateTest`和`DeduplicateTest`必须使用这些名称并保留在`flashcards`包中。
- **Don’t change the method signatures and specifications:** the public methods `toBucketSets()`, `getBucketRange()`, `practice()`, and `update()` must use the method signatures and the specifications that we provided.
- **不要更改方法签名和规范：**公共方法`toBucketSets()`、`getBucketRange()`、`practice()`和`update()`必须使用我们提供的方法签名和规范。
- **Don’t change the method signature or weaken the specification:** the public method `deduplicate()` must have a stronger spec than we provided.
- **不要更改方法签名或弱化规范：**公共方法`deduplicate()`必须具有比我们提供的更强的规范。
- **Don’t include illegal test cases:** the tests you implement in `BucketSetsTest`, `BucketRangeTest`, `PracticeTest`, and `UpdateTest` must respect the specifications that we provided for the methods you are testing.
- **不要包含非法测试用例：**您在`BucketSetsTest`、`BucketRangeTest`、`PracticeTest`和`UpdateTest`中实现的测试必须遵守我们为您正在测试的方法提供的规范。

[](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@aside_these_requirements)Aside from these requirements, however, you are free to add new public and private methods and new public and private classes if you wish.但是，除了这些要求之外，如果您愿意，您可以自由添加新的公共和私有方法以及新的公共和私有类。

## [](http://web.mit.edu/6.031/www/sp21/psets/ps1/#specifications)Specifications规格

[](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@before_you_start)Before you start, read the specs carefully, and take notes about what you observe. You can either read the Java source files directly in Eclipse, or read the 在你开始之前，仔细阅读规格，并记下你所观察到的。 您可以直接在Eclipse中读取Java源文件，也可以读取[**Javadoc documentation for all classes**](http://web.mit.edu/6.031/www/sp21/psets/ps1/doc/?flashcards/package-summary.html)[Javadoc为所有类留档](http://web.mit.edu/6.031/www/sp21/psets/ps1/doc/?flashcards/package-summary.html) generated from the source files.从源文件生成。

[](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@keep_mind_these)Keep in mind these facts about specifications:请记住以下关于规格的事实：

- [](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@some_specs_preconditions)**Some specs have preconditions.****一些规范有先决条件。** Recall from 从召回[the specs reading](http://web.mit.edu/6.031/www/sp21/classes/06-specifications/#specification_structure)[规格读数](http://web.mit.edu/6.031/www/sp21/classes/06-specifications/#specification_structure) that when preconditions are violated by the client, the behavior of the method is 当客户端违反先决条件时，方法的行为是_completely unspecified__完全未指明_..
    
- [](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@some_specs_underdetermined)**Some specs have underdetermined postconditions.****一些规范有欠确定的后置条件。** Recall that 回想一下那个[underdetermined postconditions](http://web.mit.edu/6.031/www/sp21/classes/07-designing-specs/#deterministic_vs_underdetermined_specs)[欠定后置条件](http://web.mit.edu/6.031/www/sp21/classes/07-designing-specs/#deterministic_vs_underdetermined_specs) allow a range of behavior. When you’re implementing such a method, the exact behavior of your method within that range is up to you to decide. When you’re writing a test case for the method, the test must allow for the full range of variation in the behavior of the implementation, because otherwise your test case is not a legal client of the spec as required above.允许一系列行为。 当您实现这样的方法时，您的方法在该范围内的确切行为由您决定。 当您为方法编写测试用例时，测试必须允许实现行为的全部变化，因为否则您的测试用例不是上述要求的规范的合法客户端。
    
- [](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@public_methods_can)**Public methods can be used anywhere.****公共方法可以在任何地方使用。** These methods are independent modules, which might be called by various parts of a flashcard system, not necessarily just the Modified-Leitner algorithm. A method implementation must be able to handle all inputs that satisfy the precondition, even if they don’t arise in the Modified-Leitner algorithm. And it may return any output that satisfies its postcondition, even if that doesn’t seem useful in the Modified-Leitner algorithm. 这些方法是独立的模块，可以由抽认卡系统的各个部分调用，不一定只是Modified-Leitner算法。 方法实现必须能够处理满足前置条件的所有输入，即使它们不在Modified-Leitner算法中出现。 它可以返回满足其后置条件的任何输出，即使这在Modified-Leitner算法中似乎没有用。
    

## [](http://web.mit.edu/6.031/www/sp21/psets/ps1/#testing)Testing测试

[](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@you_should_partition)You should partition each method’s inputs and outputs, write down your partitions in a testing strategy comment, and choose test cases to cover the partitions.您应该对每个方法的输入和输出进行分区，在测试策略注释中写下您的分区，并选择测试用例来覆盖分区。

[](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@each_method_you-ll)For each method, you’ll find its spec and implementation in a Java file in the 对于每种方法，您可以在`src` folder, and a corresponding JUnit test class file in the 文件夹，以及相应的JUnit测试类文件`test` folder. Separating implementation code from test code is a common practice in development projects. It makes the implementation code easier to understand, uncluttered by tests, and easier to package up for release.文件夹。 将实现代码与测试代码分开是开发项目中的常规。 它使实现代码更易于理解，不受测试的干扰，并且更容易打包发布。

[](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@test_class_method)The Test class for a method may already have example tests in it, which are provided as models. You are recommended to read and then throw away those example tests and write your own.方法的Test类中可能已经有示例测试，这些测试作为模型提供。 建议您阅读然后扔掉那些示例测试并编写自己的测试。

[](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@some_advice_about_testing)Some advice about testing:关于测试的一些建议：

- [](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@your_test_cases_should_be_chosen)**Your test cases should be chosen using partitioning****您的测试用例应该使用分区来选择**. This approach is explained in the . 这种方法在[reading about testing](http://web.mit.edu/6.031/www/sp21/classes/03-testing/#choosing_test_cases_by_partitioning)[阅读测试](http://web.mit.edu/6.031/www/sp21/classes/03-testing/#choosing_test_cases_by_partitioning)..
    
- [](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@include_comment_top)**Include a comment at the top of each test suite class describing your testing strategy****在每个测试套件类的顶部包含一个注释，描述您的测试策略**. Examples are shown in the . 示例显示在[reading about testing](http://web.mit.edu/6.031/www/sp21/classes/03-testing/#documenting_your_testing_strategy)[阅读测试](http://web.mit.edu/6.031/www/sp21/classes/03-testing/#documenting_your_testing_strategy)..
    
- [](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@your_test_cases_should_be_small)**Your test cases should be small and well-chosen.****您的测试用例应该很小并且选择得很好。** Don’t use a large set of data for each test. Instead, create inputs carefully chosen to test the partition you’re trying to test. 不要为每个测试使用大量数据。 相反，创建仔细选择的输入来测试您尝试测试的分区。
    
- [](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@your_tests_should)**Your tests should find bugs.****您的测试应该会发现错误。** We will grade your test cases in part by running them against buggy implementations and seeing if your tests catch the bugs. So consider ways an implementation might inadvertently fail to meet the spec, and choose tests that will expose those bugs. 我们将通过针对错误实现运行它们并查看您的测试是否捕获错误来对您的测试用例进行部分评分。 因此，请考虑实现可能无意中无法满足规范的方式，并选择将暴露这些错误的测试。
    
- [](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@your_tests_must)**Your tests must be legal clients of the spec.****您的测试必须是规范的合法客户。** We will also run your test cases against legal, variant implementations that still strictly satisfy the specs, and your test cases should not complain for these good implementations. That means that your test cases can’t make extra assumptions that are only true for your own implementation. 我们还将针对仍然严格满足规范的合法变体实现运行您的测试用例，并且您的测试用例不应该抱怨这些良好的实现。 这意味着您的测试用例不能做出仅适用于您自己的实现的额外假设。
    
    [](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@if_you_wish)If you wish to write test cases that test a stronger spec than we provide, you should put those tests in a separate JUnit test class, so that we don’t try to run them on staff implementations that only satisfy the weaker spec. For example, you could put stronger tests of 如果您希望编写测试比我们提供的更强规范的测试用例，您应该将这些测试放在一个单独的JUnit测试类中，这样我们就不会尝试在只满足较弱规范的人员实现上运行它们。 例如，你可以对`practice()` in a class called 在一个叫`MyPracticeTest` in the 在`flashcards` package in the 包装在`test` folder alongside the other JUnit test classes. The explicit exception to this rule is 与其他JUnit测试类一起的文件夹。 此规则的明确例外是`deduplicate()`, whose stronger spec tests should go in ，应该进行更强的规格测试`DeduplicateTest`..
    
- [](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@put_each_test)**Put each test case in its own JUnit method.****将每个测试用例放在自己的JUnit方法中。** This will be far more useful than a single large test method, since it pinpoints the problems in the implementation. 这将比单一的大型测试方法有用得多，因为它可以查明实现中的问题。
    
- [](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@run_testing_coverage)**Run testing coverage.****运行测试覆盖。** Even before you have implementations, it helps to 即使在您有实现之前，它也有助于[run your test suites with EclEmma](http://web.mit.edu/6.031/www/sp21/classes/03-testing/#coverage)[使用EclEmma运行您的测试套件](http://web.mit.edu/6.031/www/sp21/classes/03-testing/#coverage) to make sure all your tests actually ran. This catches the unfortunate bug where you forget to put 以确保您的所有测试都实际运行。 这抓住了你忘记放的不幸bug`@Test` in front of a method that’s intended to be a JUnit test.在打算作为JUnit测试的方法前面。
    
- [](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@avoid_static_fields)**Avoid `static` fields in test classes.****避免测试类中的`static`字段。** Any mutable data in them would be shared by all the tests, so use instance fields instead. When JUnit runs the 其中的任何可变数据都将由所有测试共享，因此请改用实例字段。 当JUnit运行`@Test` methods in a class, 类中的方法，[it creates a new instance of the test class for each method](https://junit.org/junit5/docs/current/user-guide/#writing-tests-test-instance-lifecycle)[它为每个方法创建测试类的新实例](https://junit.org/junit5/docs/current/user-guide/#writing-tests-test-instance-lifecycle)..
    
- [](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@be_careful_calling)**Be careful calling helper methods from testing code.****从测试代码中调用辅助方法时要小心。** Your test cases in, say, 你的测试案例，比如说，`BucketSetsTest.java` must not call a new helper method that you have defined in 不得调用新的辅助方法 您已在`BucketSets.java` or some other 或者其他什么`src/` class. We detach your testing code from the rest of your solution when we run your test suites against staff implementations, so your code in 类。 当我们针对人员实现运行您的测试套件时，我们会将您的测试代码与解决方案的其余部分分离，因此您的代码在`src/` will not be available to your test suites. Put helper methods needed by testing code into 将不适用于您的测试套件。 将测试代码所需的辅助方法放入`test/`..
    
- [](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@again_keep_your)**Again, keep your tests small.****同样，保持测试规模小。** Don’t use unreasonable amounts of resources (such as lists or strings of length 不要使用不合理数量的资源（例如列表或长度字符串`MAX_INT`). We won’t expect your test suite to catch bugs related to running out of resources; ）。 我们不会期望您的测试套件捕获与资源耗尽相关的错误；_every__每一个_ program fails when it runs out of resources.程序在资源耗尽时失败。
    
- [](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@add-commit-push_frequently_whenever_you_do_nontrivial_amount_work_e-g_after_writing_testing)**Add/commit/push frequently.****频繁添加/提交/推送。** Whenever you do a nontrivial amount of work – e.g. after writing a testing strategy comment; after choosing some test inputs; after writing JUnit methods and seeing that they pass – you should add/commit/push your work with git. 每当您做大量工作时——例如，在编写测试策略注释之后；在选择一些测试输入之后；在编写JUnit方法并看到它们通过之后——您应该使用git添加/提交/推送您的工作。
    

## [](http://web.mit.edu/6.031/www/sp21/psets/ps1/#implementation)Implementation实施

[](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@implement_each_method)Implement each method, and revise your implementation and your tests until all your tests pass.实施每种方法，并修改您的实现和测试，直到您的所有测试都通过。

[](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@some_advice_about_implementation)Some advice about implementation:关于实施的一些建议：

- [](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@helper_methods_if)**Helper methods.****帮助方法。** If you want to write helper methods for your implementation, then make them 如果您想为您的实现编写辅助方法，请制作它们`public static` methods of a new class, alongside the other classes. For example, you could put them in a 一个新类的方法，与其他类一起。 例如，你可以把它们放在一个`Utilities` class in the 在`flashcards` package of the 包`src` folder, and write tests for them in 文件夹，并为它们编写测试`UtilitiesTest` in 在`test` folder. Feel free to put multiple helper methods into one class.文件夹。 随意将多个辅助方法放入一个类中。
    
- [](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@don-t_call_testing)**Don’t call testing code.****不要调用测试代码。** Don’t put a helper method in the 不要在`test/` folder if you need to call it from implementation code in 文件夹，如果您需要从实现代码中调用它`src/`. Testing code should be completely detachable from the implementation, so that the implementation can be packaged up separately for deployment. We also detach your testing code when we run staff tests against your implementation. Put helper methods needed by implementation code into . 测试代码应该与实现完全可分离，以便实现可以单独打包以进行部署。 当我们针对您的实现运行人员测试时，我们还会分离您的测试代码。 将实现代码所需的辅助方法放入`src/`..
    
- [](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@eliminate_warnings_revise)**Eliminate warnings.****消除警告。** Revise your code to address all the yellow-highlighted warnings shown in Eclipse. These warnings should include both Java compiler warnings and Checkstyle warnings, because Checkstyle is enabled automatically for this problem set. 修改您的代码以解决Eclipse中显示的所有黄色突出显示的警告。 这些警告应包括Java编译器警告和Checkstyle警告，因为Checkstyle会自动为此问题集启用。
    
- [](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@check_testing_coverage)**Check testing coverage.****检查测试覆盖。** Do 做[glass box testing](http://web.mit.edu/6.031/www/sp21/classes/03-testing/#black_box_and_glass_box_testing)[玻璃箱测试](http://web.mit.edu/6.031/www/sp21/classes/03-testing/#black_box_and_glass_box_testing) and revise your tests until you have satisfactory code coverage.并修改您的测试，直到您获得满意的代码覆盖。
    
- [](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@review_your_own)**Review your own code.****查看您自己的代码。** Read your code critically with an eye to making it as SFB, ETU, and RFC as possible. 批判性地阅读您的代码，着眼于使其尽可能成为SFB、ETU和RFC。
    
- [](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@test_frequently_rerun)**Test frequently.****经常测试。** Rerun your tests whenever you make a change to your code. 每当您对代码进行更改时，都会重新运行您的测试。
    
- [](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@add-commit-push_frequently_whenever_you_do_nontrivial_amount_work_e-g_after_writing_method)**Add/commit/push frequently.****频繁添加/提交/推送。** Whenever you do a nontrivial amount of work – e.g. after writing a method body and seeing that the tests pass, or after finding a bug, adding a regression test for it, and fixing it – you should add/commit/push your work with git. 每当你做大量的工作时——例如，在编写了一个方法体并看到测试通过后，或者在找到一个bug后，为它添加一个回归测试并修复它——你应该用git添加/提交/推送你的工作。
    

[](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@after_you-ve_implemented)After you’ve implemented all the methods, you can run 实现所有方法后，您可以运行`Main.java` to see the methods used in a simulated flashcard application. 查看模拟抽认卡应用程序中使用的方法。 `Main.java` is not part of the spec of this problem set, is not used for grading, and you are free to edit it as you wish.不是此问题集规范的一部分，不用于评分，您可以根据需要自由编辑它。

## [](http://web.mit.edu/6.031/www/sp21/psets/ps1/#strengthening_deduplicate)Strengthening 加强`deduplicate()`

[](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@almost_all_specs)Almost all the specs in this problem set should be used exactly as-is, with one exception. The 这个问题集中的几乎所有规范都应该按原样使用，只有一个例外。 的[`deduplicate()`](http://web.mit.edu/6.031/www/sp21/psets/ps1/doc/flashcards/Deduplicate.html)[`deduplicate()`](http://web.mit.edu/6.031/www/sp21/psets/ps1/doc/flashcards/Deduplicate.html) method currently has a rather weak spec. You should at first use this weak spec to write some tests and a simple implementation. Make sure your tests respect the weak spec, and that your implementation passes your tests. Add/commit/push.方法目前的规范相当弱。 您应该首先使用这个弱规范来编写一些测试和一个简单的实现。 确保您的测试尊重弱规范，并且您的实现通过了您的测试。 添加/提交/推送。

[](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@then_revise_spec)Then revise the spec of 然后修改规范`deduplicate()` by editing the Javadoc comment in 通过编辑Javadoc评论`Deduplicate.java`. Your new spec should be:. 您的新规范应该是：

- [](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@stronger_than_original)stronger than the original spec比原来的规格更强
- deterministic
- 确定性
- helpful, in the sense of helping a user in realistic situations
- 有帮助，在现实情况下帮助用户的意义上

[](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@revising_spec_you)When revising the spec, you will need to carefully consider several words:修改规范时，您需要仔细考虑几个词：

[](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@similar_your_stronger)**Similar****类似**. Your stronger spec will have to completely define what “similar” means for the fronts of flashcards. At a minimum, your revised spec should require that:. 你更强的规范必须完全定义“相似”对抽认卡的正面意味着什么。 在极小点上，修订后的规范应要求：

- identical fronts are similar, as in the original weak spec
- 相同的前沿是相似的，就像在原始的弱规范中一样
- [](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@fronts_represent_same)fronts that represent the same English noun in 中代表同一英语名词的前面[singular and plural forms](https://www.grammarly.com/blog/plural-nouns/)[单数和复数形式](https://www.grammarly.com/blog/plural-nouns/) are similar. For example, if we write a card in the form 是相似的。例如，如果我们在表格中写一张卡片_“front__"正面_ / / _back”__后退”_, then:，然后：
    
    - **“sound** / bruit” and **“sounds** / bruits” have similar fronts
    - “声音/布鲁斯”和“声音/布鲁斯”有相似的正面
    - **“bus** / der Bus” and **“buses** / die Busses” have similar fronts
    - "总线/der总线"和"总线/芯片总线"有相似的前端
    - **“synergy** / working well together” and **“synergies** / corporate buzzword” have similar fronts
    - “协同效应/合作良好”和“协同效应/公司流行语”有相似的前沿
    - [](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@ray_el_rayo)**“ray****“雷** / el rayo” and /闪电”和**“rays****"射线** / los rayos” have similar fronts/“闪电”有类似的正面
        
        [](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@exceptions_general_rules)Exceptions to the general rules, and very specialized rules that apply only to a small number of English words, do not need to be part of your spec — more about that in a moment.一般规则的例外，以及只适用于少数英语单词的非常专业的规则，不需要成为你规范的一部分——稍后会详细介绍。
        
- one additional similarity rule of your choice 您选择的另一个相似性规则_not__不_ about the English language 关于英语
    - e.g. a grammatical form or spelling variation in some other language, or a rule for another learning topic that flashcards might be useful for
    - 例如，其他语言中的语法形式或拼写变化，或抽认卡可能有用的另一个学习主题的规则

[](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@groups_along_your)**Groups****团体**. Along with your stronger definition of similarity, your spec must make it unambiguous and deterministic which cards are grouped together. Keep the spec declarative. Put on your testing hat, or try drawing example card-similarity graphs, to discover ambiguity in your spec.. 除了你对相似性的更强定义之外，你的规范必须使哪些卡片组合在一起变得明确和确定性。 保持规范声明式。 戴上测试帽，或者尝试绘制示例卡片相似度图，以发现规范中的分歧。

[](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@canonical_your_spec)**Canonical****正则的**. Your spec should also define what the “canonical” card is for a group of similar-front cards, in a clear, deterministic, and helpful way, which is stronger than the original weak spec.. 你的规范还应该定义“正则的”卡片对于一组相似的正面卡片来说是什么，以一种清晰、确定性和有用的方式，比原来的弱规范更强。

[](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@information_thinking_about)**Information****信息**. Thinking about canonical cards will also involve thinking about the “information” content of cards, and how to combine information from multiple cards, deterministically.. 思考正则的卡片也会涉及到思考卡片的“信息”内容，以及如何组合多张卡片的信息，确定性。

[](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@simplicity_your_spec)For simplicity, your spec should not require the implementation to refer to a list of exceptions or a dictionary. So your spec may be unhelpfully inaccurate on irregular words that would require an exception list or dictionary lookup. For example, your spec may treat “mouse” and “mice” as not similar even though they are actually the same English noun, while it treats “these” and “theses” as similar even though they are completely different words.为简单起见，规范不应要求实现的详情可见异常列表或字典。 因此，您的规范可能对需要异常列表或字典查找的不规则单词不准确。 例如，你的规范可能会把“老鼠”和“老鼠”视为不相似，即使它们实际上是同一个英语名词，而它会把“这些”和“论文”视为相似，即使它们是完全不同的单词。

[](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@keep_mind_you)Keep in mind that you are 记住你是_strengthening__强化_ the spec, so 规格，所以`deduplicate` can still be called in all the situations it could be used in the original weak spec. For example, some of the flashcards may not be about vocabulary at all, but about math: e.g. “3x7 / 21” and “5x5 / 25”. A spec that assumed only vocabulary cards mattered, and collapsed all these weird math cards into a single canonical card, would indeed be a legal strengthening of 仍然可以在所有可以在原始弱规范中使用的情况下调用它。 例如，一些抽认卡可能根本不是关于词表的，而是关于数学的：例如“3x7/21”和“5x5/25”。 一个假设只有词表卡重要的规范，并将所有这些奇怪的数学卡折叠成一张正则的卡片，确实是对`deduplicate`, but not a helpful one to a poor 9-year-old trying to learn the multiplication table.，但对一个试图学习乘法表的可怜9岁孩子来说，这毫无帮助。

[](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@so_design_problem)So this is a design problem, and you will need to think broadly and carefully. In the criteria given for your spec above, “stronger” and “deterministic” are likely to be clear, but “helpful” is likely to involve tradeoffs and design decisions. There is no single right answer, but there are better and worse decisions.所以这是一个设计问题，你需要广泛而仔细地思考。 在上面为您的规范给出的标准中，“更强”和“确定性”可能是明确的，但“有用”可能涉及权衡和设计决策。 没有单一的正确答案，但有更好和更坏的决定。

[](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@after_you_change)After you change your spec in 在您更改规格后`Deduplicate.java`, revise your tests in ，修改您的测试`DeduplicateTest.java` and your implementation in 以及您在`Deduplicate.java` so that the implementation satisfies your stronger spec and passes your updated tests.以便实现满足您更强大的规范并通过您更新的测试。

[](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@some_advice_about_problem)Some advice about this problem:关于这个问题的一些建议：

- [](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@work_iteratively_bite)**Work iteratively.****迭代工作。** Bite off the requirements one bit at a time. First handle identical card fronts – update the spec, write tests, then implement. Then for English nouns: spec, test, implement. Then for your new rule: spec, test, implement. 一次咬掉一点需求。 首先处理相同的卡片前端-更新规范，编写测试，然后实施。 然后对于英语名词：spec，test，实施。 然后是您的新规则：规范、测试、实施。
    
- [](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@keep_your_original)**Keep your original tests if possible.****如果可能的话，保留您的原始测试。** Each time you change the spec, revise your test suite to test your new spec by updating the testing strategy and test cases in 每次更改规范时，通过更新测试策略和测试用例来修改测试套件以测试新规范`DeduplicateTest.java`. Keep the strategy and tests you wrote for the original weak staff-provided spec, so that your . 保留您为最初的弱员工提供的规范编写的策略和测试，以便您的`DeduplicateTest.java` is checking your implementation for compliance with the original weak spec as well as your stronger spec.正在检查您的实现是否符合原始的弱规范以及您的强规范。
    
- [](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@revise_specs_tests)**Revise specs and tests as needed.****根据需要修改规格和测试。** If you find while you are testing or implementing that you need to revise your spec, do so. 如果您在测试或实现时发现需要修改规范，请这样做。
    
- [](http://web.mit.edu/6.031/www/sp21/psets/ps1/#@add-commit-push_frequently_if)**Add/commit/push frequently.****频繁添加/提交/推送。** If you commit after every step of this process (spec, 如果您在此过程的每一步之后提交（规范，_commit__提交_, test, ，测试，_commit__提交_, implement, ，实施，_commit__提交_, etc.), then Git will keep track of your old versions of spec, tests, and code, so that you can refer back to them or even recover them if you have to. You don’t have to preserve your implementation of the original staff-provided weak spec in your final code, because your Git history should have it.等），然后Git将跟踪您的旧版本的规范、测试和代码，以便您可以参考它们，甚至在必要时恢复它们。 您不必在最终代码中保留原始员工提供的弱规范的实现，因为您的Git历史记录应该包含它。