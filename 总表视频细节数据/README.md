# 总表视频细节数据

总表（「最新工作版/第…版/2词源拆分总表_初中含义版.xlsx」）里每个词做视频要用、但总表里没有的数据，一个词一个文件，存在这里。
做视频的技能（本地 local-wordfilm-batch、云端的视频技能）**先查这里**：有就用；没有就汇报“缺哪些词”，用户把词发给 Claude 生成，生成好存进来再做。

## 文件

- `索引.tsv`：总表 9299 个词，每个词一行：批次、序号、单词、初中含义、`题目`（7 道题和答案：总表有 / 细节数据有 / 缺）、`画面`（画面设置：kit有（7A-U4）/ 细节数据有 / 缺）、细节数据文件。**生成一批就更新一次。**
- `词/<首字母>/<单词>.json`：一个词的细节数据（词组的空格写成下划线：`词/k/keep_track_of.json`）。

## 一个词的文件长什么样

```json
{
  "单词": "truly",
  "总表版本": "第七十九版",
  "生成": "2026-11-19 Claude（云端）",
  "插图": ["truly"],
  "题目": null,
  "画面": { "pics": [...], "flow": [...], "partPics": {...}, "distinguish": {...}, "extend": [...] },
  "检查": "还要补：都齐了（视频技能 6.35.0 起草检查）"
}
```

- `题目`：总表里这个词没有 7 道题时才有：`{"题目1": "…", …, "题目7": "…", "答案": "…"}`，**格式和总表那几列一模一样**（维护技能“练习题七列和答案列”）。做视频时拷进这一批用的总表副本，总表本身不动。总表有就是 `null`。
- `画面`：就是视频技能 `--edits` 要的那一份（`prefs` 的写法，见视频技能 `references/pictures.md`）。按视频技能的规矩，**画里的东西都要是写实插图（6.20 起不能用表情图）、每幅画都要动（6.25 起）、拼起来那一格的小图标要自己画**；视频技能自带的素材（8 个背景、几十样东西、22 个人物动作）只够一部分，多数词要**专门画插图**：插图脚本放 `插图/<名字>_art.py`，画面里用它画出来的图。
- `插图`：这个词用到的插图脚本名（放在 `插图/` 里，好几个词可以共用一个）；只用自带素材的词是空的。
- `检查`：存进来之前用视频技能起草检查过的结果，必须是“还要补：都齐了”。

## 怎么接着出题（7 道题，按总表顺序）

**新聊天直接用 `出题技能/zongbiao-questions.skill`**：发给新聊天并说“继续出题”，它会自己拉仓库、核对总表、按下面的步骤一批 100 个往下做、每批上传。

出题的规矩照维护技能 SKILL.md“练习题七列和答案列”（第 122、123、126 条）：第 1 题听力；2–5 题考「考点」和区别，题型不重复；6、7 题专考前面没考到的考点、写“考点：”；答案就是本词原形的最多 1 道；句子 12 个词以内、初中词汇、答案唯一；每道一句讲解。
1. `python3 工具/next_words.py <总表.xlsx> 8`：下一批还缺题目的词和它们的总表材料（含义、初中含义、考点、辨析）。
2. 写原稿：一次 100 个词时用简写稿（`工具/compact.py` 开头写了格式：一道题一行，`听|单|形|选|改|完` 开头，“|”分隔，选项用“ / ”分隔，选择题第一个选项写对的），`python3 工具/compact.py 简写稿.txt 原稿.json` 转成 json；材料用 `next_words.py 总表 100 --brief`（一个词两行）。也可以直接照 `工具/lianxi.py` 原稿的写法写 `{单词: [7 道]}` 的 json。
3. `python3 工具/save_questions.py <原稿.json> <总表版本>`：写成总表那两种格、体检（0 个问题才存）、存进 `词/`、更新索引。
4. 马上传到仓库（一批一传，防止对话中断丢掉）。

## 进度

| 日期 | 批次 | 词数 | 说明 |
|---|---|---|---|
| 2026-11-19 | 建立 | 0 | 建文件夹和索引：9299 个词里，题目和画面都缺 9259 个；7A-U4 的 29 个两样都有（画面在视频技能的 kit 里）；另外 11 个总表有题目、缺画面 |
| 2026-11-19 | 题目第 1 批 | 8 | 按总表顺序：make、made、maker、make-up、makeover、made-up、match、matching（7 道题和答案，体检 0 个问题） |
| 2026-11-19 | 题目第 2 批 | 8 | rematch、mismatch、matcher、makeup、shoemaker、mason、shoe、people（体检 0 个问题）。注意：总表 makeup 的考点写“复数 makeups”，化妆品的意思一般不可数，题目里没考这一条，以后可以用维护技能核对 |
| 2026-11-19 | 题目第 3 批 | 8 | popular、popularity、popularise、popularize、popularly、unpopular、population、populate（体检 0 个问题） |
| 2026-11-19 | 题目第 4 批 | 8 | populated、populous、public、publicly、publicize、publish、publishing、publisher（体检 0 个问题；前 4 个总表没写考点，照规矩考意思、搭配和形近词） |
| 2026-11-19 | 题目第 5 批 | 100 | publication … day（按总表顺序 100 个词，改用简写稿一次写 100 个；体检全部 0 个问题）。累计 132 个词有题目 |
| 2026-11-19 | 题目第 6 批 | 100 | daily … wag（体检全部 0 个问题；someday、living、take 三个词“答案就是本词”超过 1 道，改了再存）。累计 232 个词有题目 |
| 2026-11-19 | 题目第 7 批 | 100 | wagon … historian（体检全部 0 个问题；no、inch 两个词改过再存）。累计 332 个词有题目 |
| 2026-11-19 | 题目第 8 批 | 100 | historic … preferable（体检全部 0 个问题；wizard、preference 两个词改过再存）。累计 432 个词有题目 |
| 2026-11-19 | 题目第 9 批 | 100 | refer … whenever（体检全部 0 个问题；born、kept 两个词改过再存）。累计 532 个词有题目 |
| 2026-11-19 | 题目第 10 批 | 100 | where … adverbial（体检全部 0 个问题；quotation、without、playfully、shortly 改过再存）。累计 632 个词有题目 |
| 2026-11-19 | 题目第 11 批 | 100 | verbal … better（体检全部 0 个问题）。累计 732 个词有题目 |
| 2026-11-19 | 题目第 12 批 | 100 | best … partner（体检全部 0 个问题）。累计 832 个词有题目 |
| 2026-11-19 | 题目第 13 批 | 100 | partnership … plain（体检全部 0 个问题）。累计 932 个词有题目 |
| 2026-11-19 | 题目第 14 批 | 100 | explain … everyone（体检全部 0 个问题）。累计 1032 个词有题目 |
| 2026-11-19 | 题目第 15 批 | 100 | call … specialty（体检拦下 city 一个词：答案就是本词有 2 道，改好后放进第 16 批开头再存）。累计 1131 个词有题目 |
| 2026-11-19 | 题目第 16 批 | 100 | city … strongly（体检拦下 spicy（答案就是本词 2 道），改好后放进后半批再存，最后全部 0 个问题）。累计 1231 个词有题目 |
| 2026-11-19 | 题目第 17 批 | 100 | strength … interface（体检拦下 knowledgeable（答案就是本词 2 道），改好再存，全部 0 个问题）。累计 1331 个词有题目 |
| 2026-11-19 | 题目第 18 批 | 100 | face-to-face … simple（体检拦下 hand、indirectly（答案就是本词超过 1 道），改好再存，全部 0 个问题）。累计 1442 个词有题目 |
| 2026-11-19 | 题目第 19 批 | 100 | simply … election（体检拦下 application（答案就是本词 2 道），改好再存，全部 0 个问题）。累计 1542 个词有题目 |
| 2026-11-19 | 题目第 20 批 | 100 | intelligence … microreactor（体检拦下 object（答案就是本词 2 道），改好再存，全部 0 个问题）。累计 1649 个词有题目 |
| 2026-11-19 | 题目第 21 批 | 100 | non-reactive … relief（体检拦下 exactly、belief（答案就是本词 2 道、选项重复），改好再存，全部 0 个问题）。累计 1775 个词有题目 |
| 2026-11-20 | 题目第 22 批 | 100 | relieve … infinitive（体检全部 0 个问题）。累计 1875 个词有题目 |
| 2026-11-20 | 题目第 23 批 | 100 | engineer … upon、upwards … informative（体检全部 0 个问题）。累计 1975 个词有题目 |
| 2026-11-20 | 题目第 24 批 | 100 | informational … twig（体检全部 0 个问题；outgrew、human-tech、rejoin 各有一句超过 12 个词，改短后重存）。累计 2075 个词有题目 |
| 2026-11-20 | 题目第 25 批 | 100 | twenty … alarming（体检全部 0 个问题）。累计 2175 个词有题目 |
| 2026-11-20 | 题目第 26 批 | 100 | alarmingly … underneath（体检全部 0 个问题）。累计 2275 个词有题目 |
| 2026-11-20 | 题目第 27 批 | 100 | close … connect（outrun 答案就是本词 2 道，改过再存；其余体检 0 个问题）。累计 2375 个词有题目 |
| 2026-11-20 | 题目第 28 批 | 100 | connected … immune（体检全部 0 个问题）。累计 2475 个词有题目 |
| 2026-11-20 | 题目第 29 批 | 100 | municipal … setup（体检全部 0 个问题；discussable、proud、proudly 多写一道、improve 第 6 题漏考点，改过再存）。累计 2575 个词有题目 |
| 2026-11-20 | 题目第 30 批 | 100 | upset … problem-solving（体检全部 0 个问题；broke 答案就是本词 2 道，改过再存）。累计 2675 个词有题目 |
| 2026-11-20 | 题目第 31 批 | 100 | Pacific … voice-controlled（体检全部 0 个问题；PC 选项大小写重复、accountable 和 fairness 答案就是本词 2 道，改过再存）。累计 2775 个词有题目 |
| 2026-11-20 | 题目第 32 批 | 100 | vocabulary … mountaintop（体检全部 0 个问题；officer 答案就是本词 2 道，改过再存）。累计 2875 个词有题目 |
| 2026-11-20 | 题目第 33 批 | 100 | amount … socially（体检全部 0 个问题）。累计 2975 个词有题目 |
| 2026-11-20 | 题目第 34 批 | 100 | socialism … occasion（体检全部 0 个问题；acceptable、captivated 改过再存）。累计 3075 个词有题目 |
| 2026-11-20 | 题目第 35 批 | 100 | occasionally … shape（体检全部 0 个问题；musical、landing、inland、worry、worried、worriedly、behind、unsure、inaccurate 改过再存）。累计 3175 个词有题目 |
| 2026-11-20 | 题目第 36 批 | 100 | shaped … absence（体检全部 0 个问题；continuously、continuity、continent、tennis 改过再存）。累计 3275 个词有题目 |
| 2026-11-20 | 题目第 37 批 | 100 | absent … wrongly（体检全部 0 个问题；holy 改过再存）。累计 3375 个词有题目 |
| 2026-11-20 | 题目第 38 批 | 100 | right-and-wrong … image（体检全部 0 个问题；envelope、leader、imaginable 答案就是本词 2 道，改过再存）。累计 3475 个词有题目 |
| 2026-11-20 | 题目第 39 批 | 100 | imaging … commit（体检全部 0 个问题；longevity 答案就是本词 2 道，改过再存）。累计 3575 个词有题目 |
| 2026-11-20 | 题目第 40 批 | 100 | commitment … rating（体检全部 0 个问题；Rd 选项大小写重复、neuroscience 答案就是本词 2 道，改过再存）。累计 3675 个词有题目 |
| 2026-11-20 | 题目第 41 批 | 100 | ration … ecological（体检全部 0 个问题；portrait、site、dark 答案就是本词 2 道，改过再存）。累计 3775 个词有题目 |
| 2026-11-20 | 题目第 42 批第 1 组 | 25 | ecology … telegraph（上个对话已上传，补记）。累计 3800 个词有题目 |
| 2026-11-20 | 题目第 42 批第 2 组 | 25 | infographic … befriend（体检全部 0 个问题；fallen 答案就是本词 2 道，改过再存）。累计 3825 个词有题目 |
| 2026-11-20 | 题目第 42 批第 3 组 | 25 | e-friend … encircle（体检全部 0 个问题）。累计 3850 个词有题目 |
| 2026-11-20 | 题目第 42 批第 4 组 | 25 | circuit … refresh（体检全部 0 个问题）。累计 3875 个词有题目 |
| 2026-11-20 | 题目第 43 批第 1 组 | 25 | refreshing … beautifully（体检全部 0 个问题）。累计 3900 个词有题目 |
| 2026-11-20 | 题目第 43 批第 2 组 | 25 | beautify … ingredient（体检全部 0 个问题）。累计 3925 个词有题目 |
| 2026-11-20 | 题目第 43 批第 3 组 | 25 | aggressive … escape（体检全部 0 个问题）。累计 3950 个词有题目 |
| 2026-11-20 | 题目第 43 批第 4 组 | 25 | cap … accustom（体检全部 0 个问题）。累计 3975 个词有题目 |
| 2026-11-20 | 题目第 44 批第 1 组 | 25 | customize … sunset（体检全部 0 个问题）。累计 4000 个词有题目 |
| 2026-11-20 | 题目第 44 批第 2 组 | 25 | sunrise … excessive（体检全部 0 个问题）。累计 4025 个词有题目 |
| 2026-11-20 | 题目第 44 批第 3 组 | 25 | exceed … literal（体检全部 0 个问题）。累计 4050 个词有题目 |
| 2026-11-20 | 题目第 44 批第 4 组 | 25 | literally … print（体检全部 0 个问题）。累计 4075 个词有题目 |
| 2026-11-20 | 题目第 45 批第 1 组 | 25 | printing … criticism（体检全部 0 个问题）。累计 4100 个词有题目 |
| 2026-11-20 | 题目第 45 批第 2 组 | 25 | critique … drafty（体检全部 0 个问题；among 答案就是本词 2 道，改过再存）。累计 4125 个词有题目 |
| 2026-11-20 | 题目第 45 批第 3 组 | 25 | drag … compost（体检全部 0 个问题）。累计 4150 个词有题目 |
| 2026-11-20 | 题目第 45 批第 4 组 | 25 | posture … opponent（体检全部 0 个问题）。累计 4175 个词有题目 |
| 2026-11-20 | 题目第 46 批第 1 组 | 25 | component … groundwater（体检全部 0 个问题）。累计 4200 个词有题目 |
| 2026-11-20 | 题目第 46 批第 2 组 | 25 | ground-covering … wildfire（体检全部 0 个问题）。累计 4225 个词有题目 |
| 2026-11-20 | 题目第 46 批第 3 组 | 25 | wildflower … starlight（体检全部 0 个问题）。累计 4250 个词有题目 |
| 2026-11-20 | 题目第 46 批第 4 组 | 25 | disaster … patiently（体检全部 0 个问题）。累计 4275 个词有题目 |
| 2026-11-20 | 题目第 47 批第 1 组 | 25 | patience … manageable（体检全部 0 个问题）。累计 4300 个词有题目 |
| 2026-11-20 | 题目第 47 批第 2 组 | 25 | time-management … refusal（体检全部 0 个问题）。累计 4325 个词有题目 |
| 2026-11-20 | 题目第 47 批第 3 组 | 25 | confuse … such（体检全部 0 个问题；ocean、such 答案就是本词 2 道，改过再存）。累计 4350 个词有题目 |
| 2026-11-20 | 题目第 47 批第 4 组 | 25 | million … fearful（体检全部 0 个问题）。累计 4375 个词有题目 |
| 2026-11-20 | 题目第 48 批第 1 组 | 25 | fearless … below（体检全部 0 个问题；lower 答案就是本词 2 道，改过再存）。累计 4400 个词有题目 |
| 2026-11-20 | 题目第 48 批第 2 组 | 25 | low-lying … climber（体检全部 0 个问题；low-cost 答案就是本词 2 道，改过再存）。累计 4425 个词有题目 |
| 2026-11-20 | 题目第 48 批第 3 组 | 25 | century … glass（体检全部 0 个问题）。累计 4450 个词有题目 |
| 2026-11-20 | 题目第 48 批第 4 组 | 25 | glasses … seriousness（体检全部 0 个问题；gold、goldfish、golden-haired、seriousness 答案就是本词 2 道，改过再存）。累计 4475 个词有题目 |
| 2026-11-20 | 题目第 49 批第 1 组 | 25 | remain … year（体检全部 0 个问题；firewood 答案就是本词 2 道，改过再存）。累计 4500 个词有题目 |
| 2026-11-20 | 题目第 49 批第 2 组 | 25 | yearly … desktop（体检全部 0 个问题）。累计 4525 个词有题目 |
| 2026-11-20 | 题目第 49 批第 3 组 | 25 | disc … compact（体检全部 0 个问题；DJ、each、proven、proofread、risen 答案就是本词 2 道，改过再存）。累计 4550 个词有题目 |
| 2026-11-20 | 题目第 49 批第 4 组 | 25 | compactor … celebratory（体检全部 0 个问题）。累计 4575 个词有题目 |
| 2026-11-20 | 题目第 50 批第 1 组 | 25 | soft … cell（体检全部 0 个问题）。累计 4600 个词有题目 |
| 2026-11-20 | 题目第 50 批第 2 组 | 25 | cellar … alienate（体检全部 0 个问题）。累计 4625 个词有题目 |
| 2026-11-20 | 题目第 50 批第 3 组 | 25 | alternative … disturbing（体检全部 0 个问题）。累计 4650 个词有题目 |
| 2026-11-20 | 题目第 50 批第 4 组 | 25 | disturbance … dealt（体检全部 0 个问题）。累计 4675 个词有题目 |
| 2026-11-19 | 试做 truly | 0 | 只用自带素材加表情图拼画面：起草检查不通过（表情图要换成写实插图、每幅画都要动、小图标要自己画）。结论：多数词要专门画插图，每个词的工作量和 7A-U4 一样；全表 9299 个词做不完，要先定先做哪些（等用户定） |

## 总表里看着不对的地方（出题时绕开了，以后用维护技能核对）

- makeup：考点写“复数 makeups”，“化妆品”的意思一般不可数。
- reusables：考点写“复数 reusableses（加 es）”，reusables 本身就是复数。
- usage：初中含义写成“v. 使用”，考点里有 usaged、usaging 这样的动词变形；usage 只是名词。
- timekeeper：初中含义只写“钟表”，常见意思还有“计时员”“守时的人”。
- Mr：考点写“复数 mrs”（Mr 的复数是 Messrs）；Mrs、Ms：考点写“一般不可数，不加 s”。
- organism：考点写“不可数，没有复数”，其实常用复数 organisms。
- day：初中含义写成“天一”；nano-surgeon：初中含义写到“纳米外科医生：在细胞”就断了。
- needed：考点写 neededs、neededed、neededing；thought：考点写 thoughted、thoughting（thought 是名词，也是 think 的过去式）；mistook：考点写 mistooks、mistooked。
- other：考点写比较级 otherer、最高级 otherest；another：写 more another、most another（都没有比较级）。
- half-life：考点写复数 half-lifes（一般写 half-lives）；星期几的复数写成小写 saturdays、tuesdays……（应大写 Saturdays）；Wed.、Mon.、Fri. 写“一般不可数”。
- open-weight：写成名词、复数 open-weights（其实是形容词“开放权重的”）；takeoff：初中含义只写“模仿、恶搞”，常见意思“起飞”没写。
- wagon：初中含义只写“儿童的小拉车”；voyage：初中含义只写“太空旅行”（常见意思是坐船或乘飞船的长途旅行）。
- lone、any、no、later：考点写了不存在的比较级（loner、anier、noer、laterer……）；changed、oversaw：考点写了 changeds、oversawed 这样的变形。
- ones：初中含义写“数字1的复数”，考点写“一般不可数”；latest、findings：考点写“一般不可数”；seen：初中含义只写“被看作”。
- sightseeing：易混一栏写成“tour 是“””，后半句缺了；uneven：词性转换写成“even adv.甚至”（应是 even adj. 平的）。
- video-called、gotten：考点写了 video-calleds、gottened 这样的变形；witness：考点写“不可数，没有复数”（其实常用 witnesses）；starter：初中含义只写“发令员”（常见意思还有“头盘”“首先 for starters”）；comprehensive：考点写“一般不可数”（它是形容词）。
- bore：初中含义写“承受了；生了”（bear 的过去式），考点却写 bores、bored、boring（那是“使厌烦”的 bore），两个词混在一起；题目两个意思都考到了。
- feelings、unborn、sportsmen：考点写“一般不可数，不加 s”；relative：考点写“比较级 more relative”；best-kept：初中含义写成名词“鲜为人知的好东西”（一般是形容词，常见搭配 best-kept secret）。
- neither：变形写了比较级 neitherer、最高级 neitherest，neither 没有比较级、最高级
- either：变形写了比较级 eitherer、最高级 eitherest，either 没有比较级、最高级
- which：初中含义写成“adj. 那一个”，应是“哪一个”；变形写了比较级 whicher、最高级 whichest，which 没有比较级、最高级
- qualifications：变形写成“不可数，没有复数”，qualifications 本身就是 qualification 的复数
- plaything：变形写成“不可数，没有复数”，其实是可数名词，复数 playthings
- gameplay：变形写复数 gameplays，gameplay 一般不可数
- told：初中含义只写“v. 过去分词”，它也是 tell 的过去式
- platformed：变形是编造的（platformeds、platformeded、platformeding），它本身就是 platform 的过去式、过去分词
- shortened：变形是编造的（shorteneds、shorteneded、shorteneding），它本身是 shorten 的过去式、过去分词
- shears：变形写“不可数，没有复数”不准确，shears 本身是复数形式的名词（a pair of shears），谓语用复数
- outskirts：变形写“不可数，没有复数”不准确，outskirts 本身是复数形式的名词（the outskirts），谓语用复数
- wordplay：变形写复数 wordplays，易混又说 wordplay 是总称、不可数，前后矛盾；一般作不可数
- inventor：初中含义写成“v. 发明家”，词性应是 n.
- eventually：【易混】写成 eventually / eventuell，eventuell 是德语词，不是英语；应换成 eventually / finally（eventually 强调几经周折才……）
- eventual：【易混】说明写成“eventual 是一对“假朋友””，话不通；应写 eventual（最终的、结果上的）/ final（最后一个的、不再改的）
- outcome：【易混】outcome / consequence 里 consequence 的说明是空的（偏“”），应补上“偏（不好的）后果”
- basics：【变形】写成“不可数，没有复数”，basics 本身就是复数形式（The basics are…），应写“总用复数”
- washed：【变形】编造了 washeds、washeded、washeding；washed 作形容词没有这些变形
- through：【变形】编造了比较级 througher、最高级 throughest；through 是介词，没有比较级
- goods：【变形】写成“不可数，没有复数”，goods 本身是复数形式，谓语用复数（The goods are…）；【易混】里 merchandis 少了 e
- better：初中含义写成“adv. 较好的”，词性和释义对不上；应是 adj. 较好的 / adv. 更好地
- wells：【变形】编造了 wellses、wellsed、wellsing，还写“n. 一般不可数”；wells 本身就是 well（井 / 涌出）的复数或三单，不再变形
- writings：【变形】写“不可数，没有复数”不准确，writings（著作）本身是复数形式，谓语用复数
- co-writing：【变形】编造了 co-writings、co-writinged、co-writinging；co-writing 本身是 co-write 的现在分词 / 动名词
- parcel：初中含义只写“n. 一块地”，初中最常用的是“包裹”（辨析里也说英式 parcel = 包裹），应补上
- participation：【变形】写“复数 participations”，“参与”一般不可数，不用复数
- questionable：【易混】questionable / doubtful 里 doubtful 的说明是空的（偏“”），应补“（人）拿不准的、怀疑的”
- request：【词性转换】写成 require v.需求，require 是另一个词；request 本身既是 n. 又是 v.
- requirement：【易混】requirement / requisite 里 requirement 的说明是空的（“”），应补“（被）要求达到的条件”
- tour：【易混】tour / sightseeing 里 tour 的说明是空的（“”），应补“（一次）观光游览”
- rotary：初中含义是 adj.，【变形】却写“n. 一般不可数，不加 s”，词性对不上
- control：【变形】写“英式双写 l，美式 controled”不对，英美都写 controlled；初中含义只写 n.，变形却全是动词形式，应补 v. 控制
- misunderstanding：【变形】写“不可数，没有复数”不准确，“一次误会”可数（a misunderstanding、misunderstandings）
- misunderstood：【变形】编造了 misunderstoods、misunderstooded、misunderstooding；它本身是 misunderstand 的过去式、过去分词
- understandable：【易混】understandable / comprehensible 里 comprehensible 的说明是空的（偏“”），应补“（内容）能看懂的”
- standby：【变形】编造了形容词比较级 standbier、最高级 standbiest，standby 作形容词没有比较级
- stored：【变形】编造了 storeds、storeded、storeding；stored 本身是 store 的过去式、过去分词
- storage：【变形】写“复数 storages”，“储存”一般不可数
- planned：【变形】编造了 planneds、planneded、planneding；planned 本身是 plan 的过去式、过去分词
- following：【变形】编造了 more following、most following，following 作“下列的”没有比较级
- statistics：【变形】写“不可数，没有复数”不对，statistics（统计数字）本身是复数形式，谓语用复数（The statistics show…）
- static：初中含义是 adj.，【变形】却按名词写“不可数，没有复数”，词性对不上
- everyday：【变形】编造了 more everyday、most everyday，everyday（日常的）没有比较级
- awoke：【变形】编造了 awokes、awoked、awoking；awoke 本身是 awake 的过去式（过去分词 awoken）
- scissors：【变形】写“不可数，没有复数”不对，scissors 本身是复数形式，谓语用复数（The scissors are…），“一把”说 a pair of scissors
- watchful：【词性转换】写 watch n.手表，应是 watch v.注视、当心
- list：初中含义写成“n. 目录”，初中常用义是“清单、名单”（易混里也说“目录”是 catalogue）
- increase：初中含义只写“n. 增加”，【变形】却全是动词形式，应补 v. 增加
- concrete：初中含义是 adj.，【变形】却按名词写“不可数，没有复数”，词性对不上
- spy：【易混】spy on sb / espy 里 espy 的说明是空的（“”），应补“（远远地）瞥见”
- rebuilt：【变形】编造了 rebuilts、rebuilted、rebuilting；rebuilt 本身是 rebuild 的过去式、过去分词
- be：初中含义写成“v. 是；成为(原形”，括号没写完
- been：初中含义写成“aux. v. 构成完成时的被动语态”，说法不对（been 是 be 的过去分词）；【变形】还编造了 beens、beened、beening
- beings：【变形】写“一般不可数，不加 s”不对，beings 本身就是 being 的复数
- P.E.：【变形】写“复数 p.e.s”，是编造的，P.E. 作学科名不用复数
- fore：【变形】编造了比较级 forer、最高级 forest（forest 是“森林”），fore 没有比较级
- farther：【变形】编造了 fartherer、fartherest；farther 本身就是 far 的比较级，最高级是 farthest
- premier：【变形】编造了 premierer、premierest，premier 没有比较级
- principal：初中含义漏了最常用的“n. 校长”
- strength：【变形】写“不可数，没有复数”不全面，作“长处、优点”讲可数（strengths and weaknesses）
- shoestring：初中含义写 adj.，【变形】却按名词写“不可数”；shoestring 作“鞋带”讲是可数的
- swore：【变形】编造了 swores、swored、sworing；swore 本身是 swear 的过去式（过去分词 sworn）
- consumption：【变形】写“复数 consumptions”，“消费、消耗”一般不可数
- impromptu：初中含义是 adj.，【变形】却按名词写“一般不可数”，词性对不上
- noticed：【变形】编造了 noticeds、noticeded、noticeding；noticed 本身是 notice 的过去式、过去分词
- recognisable、recognizable：初中含义里的括号没写完（“（英式拼写”“（美式拼写”）
- joyous：【变形】编造了比较级 joyouser、最高级 joyousest，应是 more joyous、most joyous
- cunning：初中含义是 adj.，【变形】却按名词写“不可数，没有复数”，词性对不上
- handsome：初中含义只写“adj. 慷慨的”，漏了最常用的“英俊的”
- railing：【变形】写“不可数，没有复数”不对，railing 可数，常用复数 railings
- riches：【变形】写“不可数，没有复数”不对，riches 本身是复数形式，谓语用复数（His riches were…）
- e-book：【变形】编造了动词形式 e-books（三单）、e-booked、e-booking，e-book 只作名词
- detective：初中含义只写 adj.“侦探的”，最常用的是 n.“侦探”；【变形】还编造了 more detective、most detective
- employment：【变形】编造了动词形式 employments、employmented、employmenting，employment 只作名词
- savings：【变形】写“不可数，没有复数”不准确，savings 本身是复数形式（My savings are…）
- unsafe：【词性转换】写成 safe n.保险柜，应是 safe adj.安全的
- safe-keep：英语一般不说 safe-keep，常用名词 safekeeping（for safekeeping）或 keep sth safe；【变形】还编造了 safe-keeped
- real-life-like：不是常用词，“逼真的”一般说 lifelike 或 realistic
- checked：【变形】编造了 checkeds、checkeded、checkeding；checked 作形容词“方格的”没有这些变形
- check-ups：【变形】写“一般不可数，不加 s”不对，check-ups 本身就是 check-up 的复数
- catalogue：初中含义写成“n. 一连串”，最常用的是“目录”
- meaning：【变形】写“不可数，没有复数”不对，一个词可以有几个 meanings，可数
- medium：【变形】编造了比较级 more medium、最高级 most medium，medium 没有比较级
- mid：【变形】编造了比较级 midder、最高级 middest，mid 没有比较级
- middlemen：【变形】写“复数 middlemens”是编造的，middlemen 本身就是 middleman 的复数
- smiling：【变形】编造了 smilings、smilinged、smilinging；smiling 本身是 smile 的现在分词
- exactly：初中含义写成“adv. 确切的”，副词应是“确切地、正好”
- examinee：初中含义的括号没写完（“（正式用语”）
- proactive：【易混】proactive / reactive 的说明话不通（“reactive 是一对：一个‘事前主动’”），应写 reactive 是“事后才反应的”
- earphone：初中含义的括号没写完（“（多为入耳式”）
- cleaners：【变形】写“一般不可数，不加 s”不对，cleaners 本身是 cleaner 的复数（也指干洗店）
- that、those、this、these：【变形】都编造了比较级、最高级（thatter、thoser、thisser、theser 等），指示代词没有比较级
- unbelieve：英语里一般不说 unbelieve，日常说 don't believe（或 disbelieve）
- lit：【变形】编造了 lits、litted、litting 和比较级 litter、littest；lit 本身是 light 的过去式、过去分词
- flashlight：初中含义写“闪光灯”，美式 flashlight 主要指“手电筒”（英式叫 torch）
- irrelevant：【词性转换】写成“in adv.在家”，和本词无关，应是 relevant adj.相关的
- less：【变形】编造了最高级 lessest，little 的最高级是 least
- least：【变形】编造了 leaster、leastest；least 本身就是 little 的最高级
- carer：初中含义标成 adj.，还编造了比较级 carerer、carerest；carer 是名词“照护者”
- finances：【变形】写“不可数，没有复数”不对，finances 本身是复数形式，谓语用复数（Her finances are…）
- infinitive：【词性转换】写成 final adj.最后的，两者无关
- unkind：【词性转换】写成 kind n.种，应是 kind adj.善良的
- native：【变形】编造了比较级 more native、最高级 most native，native 一般不比较
- gentlemen：【变形】写“复数 gentlemens”是编造的，gentlemen 本身就是 gentleman 的复数
- jobless：【变形】写“n. 一般不可数，不加 s”，jobless 是形容词“失业的”
- info：【变形】写“复数 infos”不对，info 是不可数名词
- outgrew：【变形】编了 outgrews、outgrewed、outgrewing；outgrew 本身是 outgrow 的过去式（过去分词 outgrown）
- grown-ups：【变形】写成“一般不可数，不加 s”，其实它本身就是 grown-up 的复数（可数）
- growing-up：【变形】写了复数 growing-ups，这个名词一般不用复数
- grassroots：【变形】写成名词“一般不可数，不加 s”，其实名词 the grassroots 本身是复数形式；初中含义写的是形容词“基层的”
- joint：【变形】编了形容词比较级 jointer、最高级 jointest；joint 作形容词（联合的）没有比较级
- shouldn't：【变形】编了 shouldn'ts、shouldn'ted、shouldn'ting；情态动词没有三单和过去分词
- height：【变形】写成“不可数，没有复数”，其实可数（复数 heights，如 be afraid of heights）
- binoculars：【变形】写成“不可数，没有复数”，其实它是复数形式名词（a pair of binoculars，谓语用复数）
- doubtful：【易混】括号里写成 doubtful 偏“”，引号里漏了内容
- double：【变形】编了形容词比较级 doubler、最高级 doublest；double 没有比较级
- dual：【变形】编了比较级 dualer、最高级 dualest；dual 没有比较级
- undergone：【变形】编了 undergones、undergoned、undergoning；undergone 本身是 undergo 的过去分词
- underwent：【变形】编了 underwents、underwented、underwenting；underwent 本身是 undergo 的过去式
- forgave：【变形】编了 forgaves、forgaved、forgaving；forgave 本身是 forgive 的过去式
- forgiven：【变形】编了 forgivens、forgivened、forgivening；forgiven 本身是 forgive 的过去分词
- give-and-take：【变形】写了复数 give-and-takes，这个词一般不可数
- due：【变形】编了比较级 duer、最高级 duest；due 没有比较级
- fewer：【变形】编了 fewerer、fewerest；fewer 本身是 few 的比较级（最高级 fewest）
- art：【变形】写成“不可数，没有复数”，其实有 the arts（文艺、文科）、an art（一门技艺）这样的可数用法
- coordinates：【变形】写成“不可数，没有复数”，其实它本身是 coordinate 的复数（可数，谓语用复数）
- career-exploration：【变形】写了复数 career-explorations，这个词一般不可数
- plus：【变形】编了形容词比较级 plusser、最高级 plussest；plus 没有比较级
- filled：【变形】编了 filleds、filleded、filleding；filled 本身是 fill 的过去式、过去分词
- fulfilled：【变形】编了 fulfilleds、fulfilleded、fulfilleding；fulfilled 本身是 fulfil 的过去式、过去分词
- filling：【变形】写成“不可数，没有复数”，作“补牙的填料、馅”时可数（two fillings）
- fulfil / fulfillment：英式 fulfil 配美式 fulfillment，拼写混用；英式应是 fulfilment，美式是 fulfill
- included：【变形】编了 includeds、includeded、includeding；included 本身是 include 的过去式、过去分词
- teachings：【变形】写成“一般不可数，不加 s”，其实它本身是复数形式（谓语用复数）
- past：【变形】编了比较级 paster、最高级 pastest；past 没有比较级
- passer-by / passers-by：【变形】写成“一般不可数，不加 s”，其实可数，passer-by 的复数是 passers-by
- depth：考点写“不可数，没有复数”，但 in the depths of（在……深处）常用复数 depths，“深度”作具体数值时也可数
- personnel：考点写“不可数，没有复数”；更准确是集合名词，作主语时谓语常用复数（The personnel are …）
- dive-bombed：变形写成 dive-bombeds、dive-bombeded、dive-bombeding（不存在）；原形是 dive-bomb，变形 dive-bombs / dive-bombed / dive-bombing
- on：变形写成“adj. 比较级 oner、最高级 onest”（不存在），on 没有比较级
- clearing：考点写“不可数，没有复数”，但“林中空地”义可数（a clearing in the forest）
- unclear：词性转换写“clear v.清除”，对应关系不当，应为 clear adj.清楚的
- smarter：变形写成“比较级 smarterer、最高级 smarterest”（不存在）；smarter 本身就是 smart 的比较级
- possible：变形写“比较级 more possible、最高级 most possible”，possible 一般不分等级，初中不宜考
- sided：变形写成“v. 三单 sideds；过去式 sideded；现在分词 sideding”（不存在）；sided 是形容词，动词 side（with）的变形是 sides / sided / siding
- inside：易混写成“inside / of an hour（of an hour 是‘不到一小时’）”，内容残缺，应为 inside / within（inside an hour 不到一小时）
- settler：易混“immigrant 是“””引号里内容缺失，应为“（从外国）移入的移民”
- transition：易混“shift 偏“””引号里内容缺失
- exit：辨析把 export 列为“出口”的同义词，但 export 是货物出口，不是出口通道；初中含义“v. 离别”也不准确，应为“离开、退出”
- in：词性转换写“irrelevant adj.不相关的”，和 in 没有词性转换关系
- inward：词性转换写“in adv.在家”，对应不当
- fraction：辨析把 score、mark 列为“分数”的同义词，但它们是考试得分，fraction 是数学里的分数
- unbroke：初中含义按日常词收录，但 unbroke 只见于旧诗文，日常一律说 unbroken；出题时按 unbroken 考
- payable：变形写成“比较级 payabler、最高级 payablest”（不存在），payable 没有比较级
- PC：复数写成小写“pcs”，应为 PCs
- unfair：词性转换写“fair n.集市”，应为 fair adj.公平的
- guidelines：考点写“一般不可数，不加 s”，但 guidelines 本身就是 guideline 的复数，可数
- online：变形写“比较级 more online、最高级 most online”，online 一般不分等级
- vocabulary：考点写“不可数，没有复数”，但可说 a large vocabulary、vocabularies（可数）；不说 many vocabularies 是指“很多单词”
- official：初中含义写成“n. (公司 adj. 官方的”，括号残缺
- facility：初中含义只写“熟练、天赋”，漏了最常用的“设施（复数 facilities）”
- effective：易混“effective / valid”的说明里例子 a valid ticket 放错了位置，句子残缺
- prefect：初中含义只写“长官”，英国学校里常指“学生长、班干部”
- redid：变形写成“三单 redids；过去式 redided；现在分词 rediding”（不存在）；redid 本身是 redo 的过去式（redo / redoes / redid / redone / redoing）；初中含义“改做；重新装修）的过去式”括号残缺
- some：变形写成“比较级 somer、最高级 somest”（不存在）
- someone：易混写成“someone / their（someone 是单数，their 指代））”，内容残缺
- devastation：初中含义只写“极度的悲痛”，主要意思是“破坏、毁灭”
- enough：变形写“比较级 more enough、最高级 most enough”（不存在）
- cover：易混写成“cover the cost / miles”，不是一组易混词，说明残缺
- coverage：变形写“复数 coverages”，但辨析又说 coverage 不可数，前后矛盾
- having：变形写成“三单 havings；过去式 havinged；现在分词 havinging”（不存在），having 本身就是 have 的 -ing 形式
- must：变形写成“三单 musts；过去式 must；现在分词 musting”，情态动词没有这些变形（名词 a must 的复数才是 musts）
- cloth：易混写成“cloth / fabrique（fabrique 是工厂）”，fabrique 不是英语词，应为 cloth / fabric（面料）
- decay：只写了 n.（不可数），decay 也常作动词“腐烂、衰败”，应补 v. 三单 decays；过去式、过去分词 decayed；现在分词 decaying
- practise：词性写成 n. 练习；实践，practise 是英式动词“练习”（名词写 practice），应改为 v.
- science-music：词典里查不到，不是英语常用词，像是自造的复合词；按“科学与音乐结合的”出了题，建议核实是否保留
- holding：初中含义写“租种的土地 / 暂时存放的”太偏；常见用法是 hold 的现在分词，名词“持有的股份、财产”常用复数 holdings，写成“不可数，没有复数”不对
- hind：写了“比较级 hinder、最高级 hindest”，是编造的；hind（后面的）没有这样的比较级（hinder 是动词“阻碍”，只有 hindmost 这类说法），应删掉
- contents：变形写“不可数，没有复数”不对；contents 本身就是复数形式（the contents of the box are …；table of contents 目录），作主语谓语用复数
- present：作“出现的、在场的”时一般不用比较级，写“比较级 more present、最高级 most present”不妥
- pieced：变形写成“三单 pieceds；过去式、过去分词 pieceded；现在分词 pieceding”，是编造的；pieced 本身是 piece 的过去式、过去分词，原形变形应为 pieces / pieced / piecing
- disadvantage：易混一栏写“drawback 是“””，后半句是空的，应补“drawback 是（某事物的）缺点、弊端”
- holiness：短语“His Holiness 陛下”不对，“陛下”是 Your/His Majesty；His Holiness 是对教皇等宗教领袖的尊称（圣座、教皇陛下）
- healthcare：初中含义“医疗保健、卫生保健（英式多连写”少了右括号
- spacecraft：变形写“复数 spacecrafts”不对；craft 作“飞行器”讲复数不加 s（辨析里自己也这样说），复数仍是 spacecraft
- dollar：初中含义“元(美国”少了右括号
- ads：标成“一般不可数，不加 s”不对；ads 本身就是 ad 的复数
- controversial：易混一栏写“disputed 侧重“””，后半句是空的，应补“（事实、归属）有争执的”
- worth：初中含义写 prep.，考点却写“adj. 比较级 worther、最高级 worthest”，worth 没有比较级、最高级。
- void：考点写“比较级 voider、最高级 voidest”，void 没有比较级；【易混】void / vacant 引号里的解释是空的（“void 偏“”，vacant 是“””）。
- right-and-wrong：考点写“复数 right-and-wrongs”；“是非”一般写 right and wrong，不加连字符，也不用复数。
- spent：考点里有 spents、spented、spenting、spenter、spentest 这些不存在的变形（spent 是 spend 的过去式、过去分词，作形容词“精疲力竭的”没有比较级）。
- spend：初中含义写成“v. 度过；花费(钱”，括号没有关上，应是“花费（钱）”。
- overspend：考点写“过去式、过去分词 overspended”，应是 overspent；还写了“n. 不可数”，overspend 一般只作动词。
- spun：考点里有 spuns、spunned、spunning、spunner、spunnest 这些不存在的变形（spun 是 spin 的过去式、过去分词）。
- kitchen：考点【易混】kitchen / cuisine 的解释在“两词很可能是同一个拉丁词走”处断了（辨析里是完整的“走两条路进的英语”）。
- belong：初中含义只写“v. 属”，应是“属于”。
- belongings：考点写“不可数，没有复数”；belongings 本身是复数名词，谓语用复数（My belongings are …）。
- length：考点写“不可数，没有复数”；length 是可数名词，常用复数 lengths（different lengths）。
- conductor：初中含义只写“导体；避雷针”，漏了最常用的“指挥”和“售票员、列车员”（辨析里提到了）；“避雷针”是 lightning conductor。
- outdoor、indoor：考点写“比较级 more outdoor / more indoor、最高级 most outdoor / most indoor”，这两个词不分等级，没有比较级、最高级。
- mishear：考点写“过去式、过去分词 misheared”，应是 misheard。
- overheard：考点里有 overheards、overhearded、overhearding 这些不存在的变形（overheard 是 overhear 的过去式、过去分词）。
- invited：考点里有 inviteds、inviteded、inviteding 这些不存在的变形。
- Rd：考点写“复数 rds”；Rd 只是书写缩写，复数写 Rds 也很少见。ride：初中含义写成“骑(马”，括号没关上。
- outing：初中含义写成“揭露他人的同性恋身份”，初中应取最常用的“出游、远足”（考点和辨析讲的也是出游）。
- digitalization：辨析里的【同义词】写成“企业：digitalization、enterprise、business、concern”，和“数字化”无关。
- training：考点写“复数 trainings”，辨析却写“training 不可数”，前后矛盾；一般不可数。
- trainers：考点写“一般不可数，不加 s”；trainers（运动鞋）本身是复数名词，说 a pair of trainers。
- darken：考点【易混】darken / dim 的解释写成“dim 是“””，引号里是空的（辨析里有：dim 是灯光调暗、变模糊）。
- effort：初中含义只写“n. 精力”，最常用的“努力”没写（辨析里写了）。
- eco：初中含义写 n.、考点写“复数 ecos”；eco 一般只作前缀或形容词（eco products），不作可数名词。
- reprogrammed：考点里有 reprogrammeds、reprogrammeded、reprogrammeding 这些不存在的变形；reprogramme 的考点写“去 e 加 d”，其实是双写 m（reprogrammed、reprogramming）。
- mechanism：变形写“不可数，没有复数”，实际可数，复数 mechanisms（出题按可数）。
- freshmen：它本身就是 freshman 的复数，变形却写“一般不可数，不加 s”，应改成“freshman 的复数”。
- 6th-graders、7th-graders：本身是复数，变形却写“一般不可数，不加 s”，应改成“6th-grader / 7th-grader 的复数”。
- above：变形写“比较级 more above、最高级 most above”，above 没有比较级，应删去。
- heading、subheading：变形写“不可数，没有复数”，实际可数，复数 headings、subheadings。
- headquarters：写成“不可数，没有复数”，实际是单复同形的可数名词（a headquarters / two headquarters），谓语单复数都可以。
- cattle：写成“不可数，没有复数”，实际是复数名词，谓语用复数（The cattle are …），不能说 a cattle。
- customs：本身带 s，变形写“一般不可数，不加 s”容易误导，建议写“作‘海关’时总是用 customs，谓语单复数都可”。
- solar-energy：英语写作两个词 solar energy、一般不可数，总表加了连字符，还写“复数 solar-energies”，应改。
- tongue-twister：易混写成“tongue-tied 是“””，引号里的释义空着，应补“（紧张得）说不出话的”。
- pressing：初中含义有 n. 压制品（唱片等），这个意思可数（复数 pressings），总表写成“n. 不可数，没有复数”不对
- plastic：总表写“n. 不可数，没有复数”，但泛指各类塑料、塑料制品时常用复数 plastics（the plastics industry），宜改成“多不可数；指塑料制品可用 plastics”
- criticism：总表写“n. 不可数，没有复数”，但指具体的批评意见时可数（复数 criticisms），泛指“批评”才不可数
- Dr：复数写成 drs，应为 Drs（首字母大写，如 Drs Smith and Jones）
- mini-doctor：初中含义“小医生：如学了些健康”像是被截断，句子不完整
- document：【易混】document / file 里 file 的解释是空的（“file 是“””），缺内容
- drawings：写成“n. 一般不可数，不加 s”不对；drawings 本身就是 drawing 的复数
- withdrew：变形编造了 withdrews、withdrewed、withdrewing；withdrew 是 withdraw 的过去式，本身没有这些变形
- opposite：写了“比较级 more opposite、最高级 most opposite”，opposite 一般不分等级，没有比较级、最高级
- postwoman：复数写成 postwomans，应为 postwomen；初中含义“女邮递员（英式”少了右括号
- compound-complex：标成名词并写了复数 compound-complexes，它其实是形容词（compound-complex sentence 并列复合句），没有复数
- grounds：写成“n. 不可数，没有复数”不对；grounds（理由）本身是复数形式，谓语用复数（There are good grounds …）
- underground：短语写 take an underground，应为 take the underground；另写了比较级 more underground，“地下的”一般不分等级
- groundbreaking：是形容词，【变形】却写“n. 不可数，没有复数”
- wheat：写“复数 wheats”，wheat 一般不可数，日常不用 wheats
- springboard：写“一般不可数，不加 s”不对；跳板可数（two springboards）
- fishermen：写成“n. 一般不可数，不加 s”不对；fishermen 本身就是 fisherman 的复数
- starfish：写“一般不可数，不加 s”不对；海星可数，复数 starfish 或 starfishes
- screening：写“n. 不可数，没有复数”不对；作“放映场次”“一次筛查”时可数（three screenings）
- time-management：写“复数 time-managements”，时间管理不可数，没有复数
- manners：初中含义照抄了 manner 的“方式；态度”，manners 应是“礼貌、礼节”
- founded：变形编造了 foundeds、foundeded、foundeding；founded 本身是 found（建立）的过去式、过去分词
- servings：写成“n. 一般不可数，不加 s”不对；servings 本身就是 serving 的复数
- millennium：【变形】只写了复数 millenniums，常用复数是 millennia（辨析里写了，变形里没写）
- extras：写成“n. 一般不可数，不加 s”不对；extras 本身就是 extra 的复数（群众演员、额外的东西）
- exotic：【易混】写成 exotic dancer / strange，不适合初中材料，建议改为 exotic / foreign（exotic 带新奇、来自远方的意味，foreign 只是“外国的”）
- rosewood：初中含义写成“吉他、钢琴”，应是“花梨木、红木”（常用来做吉他、家具）
- woods：写成“n. 一般不可数，不加 s”不对；woods（树林）本身是复数形式，谓语常用复数
- lie：“谎言”义的词性标成 v.，应为 n.（tell a lie）
- glasses：写成“n. 不可数，没有复数”不对；glasses 本身是复数形式（眼镜；玻璃杯的复数）
- sunglasses：写成“一般不可数，不加 s”不对；sunglasses 本身是复数形式，谓语用复数
- goldfish：写“一般不可数，不加 s”不对；金鱼可数，复数一般就是 goldfish
- gold-leaf：初中含义只剩“作定语多加连字符）”，缺了“金箔”；变形写的复数 gold-leafs 不对，金箔作材料不可数
- rose-gold：写“复数 rose-golds”，颜色名一般不可数
- seriously：词性标成 adj.，应为 adv.（严肃地；认真地；严重地）
- fireworks：写成“n. 不可数，没有复数”不对；fireworks 本身是 firework 的复数（烟花表演），谓语用复数
- drop：【易混】drop / fall 里 fall 的解释是空的（“fall 是“””），应补“fall 是自己落下”
- front：编造了比较级 fronter、最高级 frontest；front 没有比较级（说 nearer the front）
- DJ：复数写成小写 djs，动词变形写成 djs、djed、djing，应大写为 DJs、DJ'd/DJed、DJing
- proof：编造了比较级 proofer、最高级 proofest；proof 作形容词（能抵挡…的）没有比较级
- proofread：过去式、过去分词写成“proof read”，应为 proofread（和 read 一样不变，只是读音变）
- rose：两行都写了动词变形 roses、rosed、rosing，这是编造的；rose 一行是“玫瑰”（复数 roses），另一行应是 rise 的过去式
- risen：变形编造了 risens、risened、risening；risen 是 rise 的过去分词，本身没有这些变形
- arose：变形编造了 aroses、arosed、arosing；arose 是 arise 的过去式，本身没有这些变形
- hallways：写成“n. 一般不可数，不加 s”不对；hallways 本身就是 hallway 的复数
- magic：写“复数 magics”，magic（魔术、魔法）不可数，没有复数
- cyber：编造了比较级 cyberer、最高级 cyberest，还写了“n. 一般不可数”；cyber 只作定语用，不分等级
- else：编造了比较级 elser、最高级 elsest；else 没有比较级
- rock-eater：初中含义写成说明文字（“不是词典里的固定词……”），不是释义
- sped：变形编造了 speds、spedded、spedding；sped 本身是 speed 的过去式、过去分词
- farmland：【变形】写“复数 farmlands”，辨析里又说“farmland 不可数”，前后矛盾；一般作不可数

## 总表里出现两次的词（细节数据按单词存一份，两行共用；出题时两个意思都要考到）

- bore（批次6序号66：v. 承受了；生了；批次62序号31：v. 使厌烦）
- lie（批次56序号27：v. 谎言
v. 卧；位于；批次56序号62：v. 谎言
v. 卧；位于）
- rose（批次58序号13：n. 玫瑰花；批次170序号3：n. 玫瑰花）
- race（批次64序号70：v. 比赛；批次64序号71：v. 比赛）
- scale（批次78序号44：n. 规模；等级、级别
v. 攀登；批次110序号10：n. 鳞、鳞片；水垢）
- mine（批次101序号16：pron. 我的；批次103序号22：n. 矿、矿井；地雷
v. 开采）
