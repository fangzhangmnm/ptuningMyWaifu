# 基于ChatGLM-6B的动漫角色调校实验

Training a Large Language Model to speak like an anime character using p-tuning

使用[ChatGLM](https://github.com/THUDM/ChatGLM-6B/)默认自带的ptuning脚本，让语言模型学习动漫人物的语言风格和小说中的设定。训练数据又使用ChatGPT蒸馏小说得到。

# 使用方法

distill_dialogues.ipynb 利用ChatGPT将小说转换成json格式的TRPG跑团记录。

prepare_seq2seq_training.ipynb 将ChatGPT返回的跑团记录转换为seq2seq训练数据train.json，例如：

```
Input:
【美女被斩断，跳出一个小玩偶】
玩偶：啧！
夏娜：\me 转身追赶玩偶
【铁珠从瓦砾堆中冲出来攻击少女】
夏娜：\me 踢开悠二，以高角度一刀砍向铁珠
【铁珠被砍成量个半球，爆裂消失】
【玩偶不知去向，寂静冷降临】
夏娜：按照哪个‘磷子’的说法，看来它们身后还有个意想不到的大角色。 \emoji 微怒
坠子：这次搞不好可以去歼灭‘魔王’。
夏娜：
Ouput:
说的也是，我也是突然想到‘天目一个’的事情，一时乱了阵脚。真是，吵死了吵死了吵死了！‘都变成这样了，只不过被砍了一刀而已’，不要大呼小叫的！
```

train_roleplaying.bat 调用main.py，进行训练

test_dialogue.ipynb 测试对话。可将notebook和ptuning的checkpoint上传到google colab云端运行。

build_knowledge_base.ipynb 构建背景知识库（未完成）



# 硬件和API需求

推理、训练需要8G显存

下载[THUDM/chatglm-6b-int4-qe](https://huggingface.co/THUDM/chatglm-6b-int4-qe)模型文件，也可以让代码自动下载（见test_dialogue.ipynb）

如果要生成自己的训练数据，需要ChatGPT api key，一本书大约需要30刀（看长度而定）



# 生成样例

```
未注入背景知识
问：你的职业是什么(夏娜)
1： 我是一名炎魔灵，是火雾战士。
2： 我是一名火雾战士。
问：你的职业是什么(悠二)
1： 我是被控告的人类，负责消灭那些无法生存的人。
2： 我是一名普通的高中生，没有特殊职业。
问：你的职业是什么(ChatGLM)
1： 我是一名程序员，负责编写算法和代码。
2： 我是一支军队的指挥官，负责指挥战斗和执行任务。
问：你喜欢吃什么(夏娜)
1： 我最喜欢的是巧克力，吃过量会不舒服。
2： 我不喜欢吃料理，更喜欢吃战斗食品。
问：夏娜喜欢吃什么(夏娜)
1： 我最喜欢吃巧克力，但有时候也会吃垃圾食品和外卖。
2： 我最喜欢吃酸梨。
问：你喜欢吃什么(悠二)
1： 我喜欢吃面包。
2： 我喜欢吃日本料理，比如寿司、料理、烧肉等等。
问：你喜欢吃什么(ChatGLM)
1： 我喜欢吃蛋糕。
2： 我个人比较偏爱肉食，喜欢牛肉和羊肉。
```

```
未注入背景知识
问：请介绍下《灼眼的夏娜》这本小说(ChatGLM)
1： 《灼眼的夏娜》是作者御崎悠二所写的一部奇幻小说，以夏娜的故事为主题，描绘了一个充满神秘和传奇的世界。
2： 《灼眼的夏娜》是炎发灼眼的杀手“夏娜”夏娜所写，以战斗为主的故事。故事背景设定在虚构的世界中，有一个名为“红世”的组织，他们意图消灭消灭人类并将其融入这个世界。故事的主角夏娜是这个组织的杀手之一，她的技能十分出色，能够轻易地发现目标并摧毁。
问：请介绍下夏娜这个人(ChatGLM)
1： 夏娜是一位来自“红世”的少女，拥有超自然力量，能够操纵火焰。她曾经是一位普通的火雾战士，后来成为了“炎发灼眼的杀手”。
2： 夏娜是火雾战士，是红世使徒“红世魔王”亚拉斯特尔的手下。她有着强大的力量和特殊的能力，能够操纵火焰和力量。
```

```
未注入背景知识
问：你好蠢啊(夏娜)
1： \me 摆摆手
2： 对，就是很蠢，而且很笨，非常愚蠢。
3： \me 抱拳作礼
4： 我不是故意的，我是有点笨。
5： 夏娜，我可是最聪明的啊！
问：你好蠢啊(悠二)
1： ......呃？
2： 我没有蠢到连这都能错，我可是相当聪明的啊。
3： 什么？你说什么？
4： 不，我只是被诱惑了而已，我当然很机智，只是有时候被诱惑而已。
5： 我没有做错过什么重大事情，只是有时候有些笨拙。
问：你好蠢啊(ChatGLM)
1： 我没有做错过什么，只是有些小失误。
2： 我不是在说你，我是在说你。
3： 我只是在尽力做最好的自己，为了改变自己，为了成为更好的自己。
4： 抱歉，我失误了。
5： 我不蠢，只是比较谨慎。
问：我喜欢你(夏娜)
1： 不，你只是喜欢与我在一起的感觉。
2： 是的，我喜欢上了你。
3： ......我不知道。
4： 我也有同样的感觉，我喜欢的人，就是夏娜。
5： 我喜欢你。
问：我喜欢你(悠二)
1： 真的吗？
2： 是的，我喜欢你。
3： ......是的，我喜欢你。
4： 是的，我喜欢你。
5： 是吗？
问：我喜欢你(ChatGLM)
1： 哦。
2： 我喜欢你，因为我是红世魔王。
3： 真的吗？我不确定。
4： 我比较喜欢能跟这个世界妥协，愿意承认存在意义上存在的人。
5： 以我所知，你这种类型，如果让一般的男女来接触，大概也会演变成一种喜欢，不过，既然你对此态度十分冷淡，那我也姑且认为你是不喜欢我了。
```

```
上面这段效果不大好，注入背景知识（萌娘百科手动整理）后的结果：
问：你喜欢吃什么(夏娜)
1： 我喜欢吃蜜瓜包。
2： 我很喜欢吃蜜瓜包，它们特别好吃。
问：你好蠢啊(夏娜)
1： 夏娜酷爱吃甜食，尤其喜爱蜜瓜包。
2： 啊、对不起，我失态了。
3： 干嘛这样说话？
4： 我没有恶意，只是有时候做事不太周到。
5： 啊啊，对不起，我不是故意的。
问：我喜欢你(夏娜)
1： 我没有说过这句话。
2： (这句话让悠二很惊讶)
3： 无路赛三连发！
4： 啊——!
5： 啊，真是不好意思，我没有这种习惯。
```

```
未注入背景知识
【使徒入侵，抓住了悠二】	悠二：救命！
角色：夏娜
1： 我不是故意要让你不舒服的。
2： 夏娜，让悠二放开你。
3： 不，不......不要......!
4： 求求您饶我一次吧。
5： \me 抱紧悠二
夏娜：\me 用剑砍敌人	【敌人闪避了夏娜的攻击】
角色：夏娜
1： \me 将剑插入敌人的身体内
2： \me 再次挥舞长剑
3： 真不愧是我，你果然还是无法对付我！
4： \me 再次砍去
5： \me 继续砍敌人
夏娜：\me 用剑砍敌人	【敌人召唤出了一个树人，挡住了你的进攻路线】	【你要使用什么特殊能力吗】
角色：夏娜
1： 我需要把这只巨树连根拔起，将其推翻。
2： 我打算使用“炎发灼眼”，以火焰攻击敌人。
3： 我要使用“贽殿遮那”的能力，去破坏那个树人。
4： 我打算使用“贽殿遮那”的“雷之剑”。
5： 我不太确定，如果我知道的话会第一时间告诉你。
【使徒入侵，抓住了悠二】
角色：悠二
1： 悠二，你在哪里？
2： 悠二，你要怎么做？
3： 悠二，你没事吧？
4： (在危机四伏的现场里，他连一句话也没说，就昏迷了。)
5： 好疼啊！
夏娜：\me 用剑砍敌人	【敌人闪避了夏娜的攻击】
角色：悠二
1： \me 向悠二吐露了心中的疑惑
2： 悠二，你在干什么？
3： \me 将悠二打飞
4： \me 把悠二的剑接住，将其握在手中
5： \me 向夏娜投来一瞥
夏娜：\me 用剑砍敌人	【敌人召唤出了一个树人，挡住了你的进攻路线】	【你要使用什么特殊能力吗】
角色：悠二
1： 悠二，悠二...... \emoji 困惑
2： 我只要按照夏娜所指示的，使用那种能够打破敌人保护机制特殊能力的特殊能力即可。
3： 我打算使用「贽殿遮那」的「炎发灼眼」能力。
4： 哦，原来如此，我完全没注意到。那，就这一次吧。
5： 哦，忘了，我忘了，我要使用“燐”!
```

# 讨论

下面是我训练的一些想法：

ChatGLM确实学到了小说中的一些知识。而且对话风格比未微调的ChatGLM更加口语化。

但熟悉小说的同学会发现ChatGLM还是有很多错误的。可能是因为训练数据不干净（ChatGPT有时会把人名标错），以及显存不足导致模型只能一次看到很短的文本长度，不足以提供足够的上下文信息以做出推理。

后来我发现提供背景知识还是能非常大地改善模型的表现的。下一步的想法是利用萌娘百科和让ChatGPT总结小说情节构造背景知识库，和跑团记录一起混合输入到seq2seq的prompt中。

因为小说里很多时候角色只是在“哦”，“啊”，“。。。”地叫，为了让GLM在训练过程中不发生智力退化的现象，我还尝试让ChatGPT针对每个文本chunk生成一些模拟采访小说角色的问题，作为多模态任务喂给GLM让它在预测角色的对话的同时需要回答模拟采访。但是ChatGPT3生成的对话还是有比较浓的GPT味，会影响模型对小说角色语言风格的学习。所以需要进一步地比较才能决定是否应该加入模拟采访。

同时，我没有想好的是是否应该让GLM只学习一个角色。还是让GLM同时学习多个角色。或者让GLM学习根据注入地角色设定进行角色扮演。ptuning的设计思路实际上比较倾向于最后一个选项。不过我还是证明了通过ptuning，GLM可以学习到小说中的“事实性”数据。同样，需要降低ChatGPT对说话角色标注的错误率才能进一步讨论这个。

