
# 📋 Claude Writing Execution SOP v6.0 - Human Simulation System
# Claude写作执行SOP v6.0 - 人类模拟系统

**Design Philosophy**: Characters are not puppets. They are simulations of humans with working brains, bodies, and survival instincts.

**设计哲学**：角色不是木偶。他们是有运转的大脑、身体和求生本能的人类模拟体。

---

## §0 Core Principle: The Human Machine
## §0 核心原则：人类机器

### 0.1 What Makes Writing Feel "Fake"

**Fake writing happens when**:
- Author controls character like a game character (press button → character does action)
- Character's brain is **selectively smart** (knows what author needs them to know)
- Character's body doesn't follow biology (no fatigue, pain is optional, injuries don't affect performance)
- Character's emotions are **labeled** not **experienced** ("he felt sad" vs body actually responding to sadness)

**Real writing happens when**:
- Character's brain runs its own simulation (limited information, makes mistakes, has blind spots)
- Character's body has **physical state** (hunger, pain, exhaustion affects decisions)
- Character's emotions are **physiological cascades** (fear = body response → brain interprets → action)
- Character can be **interrupted** (plans fail, reactions are messy, surprises derail thinking)

---

### 0.2 The Human Operating System

Every human runs on this stack:

```
Layer 5: PLANNING (slowest, first to shut down under stress)
  ├─ Long-term goals
  ├─ Social strategies  
  └─ Abstract reasoning

Layer 4: PROBLEM-SOLVING (moderate speed, requires focus)
  ├─ "What does this mean?"
  ├─ "What should I do?"
  └─ Conscious decisions

Layer 3: PATTERN RECOGNITION (fast, automatic)
  ├─ "I've seen this before"
  ├─ Familiar vs unfamiliar
  └─ Expectations

Layer 2: EMOTIONAL RESPONSE (very fast, involuntary)
  ├─ Body sensations (gut clench, heart race)
  ├─ Impulses (run, freeze, fight)
  └─ Facial expressions

Layer 1: SURVIVAL REFLEXES (instant, bypasses brain)
  ├─ Flinch
  ├─ Pain withdrawal
  └─ Startle response
```

**CRITICAL RULE**: Under stress, higher layers **shut down**. 

- Mild stress: Layer 5 offline, Layer 4 impaired
- High stress: Layer 5-4 offline, Layer 3 impaired  
- Extreme stress (life/death): Only Layer 2-1 functioning

**This is not optional**. This is biology.

---

### 0.3 Why Claude Writes Fake Humans

**Claude's default behavior**:
- Gives character perfect situational awareness (Layer 5 always online)
- Character "realizes" things conveniently (serves plot, not psychology)
- Emotions are labels ("felt afraid") not cascade effects
- Body is scenery, not a system with state

**What we need to train**:
- Track character's **information state** (what they actually know vs what author knows)
- Track character's **physical state** (injuries, exhaustion, sensory overload)
- Track character's **stress level** → determines which brain layers are online
- Let character be **wrong, confused, and inefficient**

---

## §1 Character State Tracking
## §1 角色状态追踪

### 1.1 Before Writing Any Scene

**Initialize character state variables**:

```python
CHARACTER_STATE = {
    # Physical
    "injuries": [],  # "left leg fractured", "bleeding from forehead"
    "exhaustion": 0-100,  # 0=fresh, 100=collapse imminent
    "pain_level": 0-10,
    "sensory_overload": 0-100,  # affects perception accuracy
    
    # Cognitive  
    "stress_level": 0-100,  # determines which brain layers work
    "available_layers": [1,2,3,4,5],  # which parts of brain are functioning
    "focus_target": "immediate threat",  # what brain is locked onto
    "confusion_level": 0-100,  # how much doesn't make sense
    
    # Information
    "knows": [],  # facts character actually has
    "assumes": [],  # things character thinks are true
    "confused_about": [],  # active questions in their mind
    "missed": [],  # things in environment they didn't notice
}
```

### 1.2 State Determines Behavior

**DO NOT write action without checking state**:

```
WRONG PROCESS:
1. Plot needs character to notice detail
2. Write "he noticed the symbol on the wall"

RIGHT PROCESS:  
1. Check: stress_level = 85 (extreme)
2. Check: available_layers = [1, 2] (only reflexes + emotion)
3. Check: focus_target = "the monster chasing me"
4. Conclusion: Character CANNOT notice symbol
5. Symbol remains in environment (maybe noticed later)
```

**Example**:

```
❌ FAKE:
"陈锋在逃跑中注意到那些符号泛着荧光。"

(Why fake? His stress_level=95, brain is in survival mode, 
he can't spare processing power for curiosity)

✅ REAL:
"陈锋的视线扫过墙面——灰、土、黑、【某个亮的东西】——他没停下。"

(He SAW it, but brain categorized as "not immediate threat" 
and discarded. This is how human perception actually works)
```

---

### 1.3 Stress Level Effects (Mandatory Reference)

| Stress | Available Layers | Behaviors | Prohibited Actions |
|--------|------------------|-----------|-------------------|
| 0-30 | All (1-5) | Can plan, analyze, notice details | None |
| 31-60 | 1-4 | Focused problem-solving, less awareness of surroundings | Complex planning, multitasking |
| 61-85 | 1-3 | Pattern-match only, "this is like X", automatic responses | Analysis, curiosity, abstract thought |
| 86-100 | 1-2 | Pure reaction, no thinking | Everything except reflexes |

**Writing Requirement**: 

Before character does ANYTHING cognitive (notices, realizes, decides), check their stress level. If action requires Layer 4+ but stress has shut it down → **rewrite the action**.

---

## §2 The Body Is Not Scenery
## §2 身体不是布景

### 2.1 Injury = Permanent State Change

**Once a character is injured, EVERY subsequent action is affected**.

```python
# Example: 陈锋 fractured his left leg at timestamp T=100

# At T=101, 102, 103... T=999:
# EVERY action must check:
if action.requires_leg_mobility():
    action.effectiveness *= 0.3  # 70% capability loss
    action.pain_caused = 7  # pain spikes with movement
    action.speed /= 3  # much slower
```

**Common mistake**:
```
❌ WRONG:
"陈锋左腿骨折，他忍着痛继续跑..."
[500字后]
"陈锋跳过障碍物..."

Problem: Forgot the injury. Body state didn't persist.
```

**Correct approach**:
```
✅ RIGHT:
"陈锋左腿骨折，他拖着腿爬..."
[500字后]  
"障碍物。他只能绕。跳不了。"

Every. Single. Action. Affected.
```

---

### 2.2 Pain Is Not A Label

**Pain is a signal that hijacks attention**.

```
❌ FAKE PAIN:
"他的腿很疼，但他忍住了，继续观察敌人的动作。"

Why fake: Pain doesn't work like this. You can't just "忍住" and 
continue complex observation. Pain FORCES attention to itself.

✅ REAL PAIN:
"敌人在动——腿传来一阵钻心的痛——草——等等，敌人去哪了？"

Pain interrupts. Focus breaks. Has to refocus. This is biology.
```

**Pain mechanics**:
- Constant pain (5-6/10): Reduces available attention by 40%
- Spike pain (7-8/10): Forces immediate attention, interrupts action
- Extreme pain (9-10/10): Shuts down Layer 3-5, only reflexes remain

---

### 2.3 Exhaustion Accumulates

**Every physical action has a cost**.

```python
exhaustion_level = 0

# Running 100m with fractured leg:
exhaustion_level += 15

# Climbing while bleeding:
exhaustion_level += 20

# When exhaustion > 80:
available_layers = [1, 2]  # brain shutdown from exhaustion
reaction_time *= 2  # everything slower
```

**Writing Rule**: If character has done intense physical activity for 3+ scenes, they MUST show exhaustion effects:

- Vision narrowing
- Breath ragged (affects speech)
- Legs shaking
- Decision-making impaired
- Emotional control weakened (might cry, rage unexpectedly)

---

## §3 Information Asymmetry: Character ≠ Author
## §3 信息不对称:角色 ≠ 作者

### 3.1 The God View Problem

**Author knows everything. Character doesn't.**

```
❌ WRONG:
"陈锋看到了修仙者，意识到这是另一个文明。"

Why wrong: How does he "意识到"? His world model is:
- Knows: Modern Earth, geology, physics
- Doesn't know: Cultivation exists, other worlds exist

Correct thought: "那特么是什么？特效？幻觉？"
```

**Character can only use what's in their knowledge base**:

```
陈锋's knowledge base:
✅ Geology, Earth science
✅ Military training (basic)
✅ 21st century physics
❌ Cultivation lore
❌ Other dimensions  
❌ Anything supernatural

When he sees flying sword:
- Brain searches knowledge base
- Finds: No match
- Output: Confusion, not understanding
```

---

### 3.2 Misinterpretation Is Mandatory

**When humans see something they don't understand, they map it to closest known thing**.

```
✅ CORRECT:
陈锋 sees 妖兽 → brain searches → finds: "wild boar" 
→ labels it "巨型野猪" (WRONG label, but brain needs a label)

Later: Gets closer → size doesn't match → brain error → 
"不，那不是猪" → recategorizes to "我特么不知道那是什么"
```

**This is how human cognition works**:
1. See unfamiliar thing
2. Brain auto-maps to familiar category (often wrong)
3. More data → mismatch detected → confusion
4. Eventually: "I don't know what this is" (anxiety spikes)

**Writing requirement**: Show this process. Don't let character instantly "know" what things are.

---

### 3.3 Confusion Is A Physical State

When something doesn't make sense, human body responds:

```
confusion_level > 50:
├─ Physiological: Dizziness, nausea, headache
├─ Emotional: Anxiety, frustration, fear
├─ Cognitive: Difficulty focusing, memory glitches
└─ Behavioral: Freeze, retreat, or aggression

confusion_level > 80:
└─ Dissociation risk (brain gives up trying to understand)
```

**Example**:
```
❌ FAKE:
"陈锋看到紫红色的天空，感到困惑。"

✅ REAL:

他看向手表——指针在乱转。看向天——还是紫的。"

(Confusion = body trying to reject impossible information)
```

---

## §4 Emotion Is Not A Label
## §4 情绪不是标签

### 4.1 Emotion = Physiological Cascade

**Emotions are body-first, interpretation-second**.

```
WRONG MODEL:
Brain thinks "I'm in danger" → Feels fear → Body responds

CORRECT MODEL:  
Stimulus → Body responds (heart races, breath shallow) 
→ Brain interprets body state → Labels it "fear"
```

**Writing implication**:

```
❌ NEVER WRITE:
"他感到恐惧。"
"她很伤心。"  
"他非常愤怒。"

✅ ALWAYS WRITE:
"他的心跳在喉咙里。" (body state)
"她的视线模糊了。" (body state)  
"他的拳头握紧，指甲刺进掌心。" (body state)

Reader's brain will label the emotion. You don't need to.
```

---

### 4.2 Emotional Contagion

**Emotions spread through physical presence**.

If character A is terrified:
- Breathing changes (other characters hear it)
- Body language changes (other characters see it)
- Voice changes (pitch, pace, tremor)

Character B will **automatically mirror** some of this (mirror neurons).

**Writing rule**: In group scenes, emotions ripple:

```
✅ REAL:
"小王的枪声停了。老赵回头——小王在发抖。
老赵的手也开始抖。"

(Fear spreads non-verbally, faster than conscious thought)
```

---

### 4.3 Emotion Overrides Cognition

**When emotion intensity > 70%, Layers 4-5 shut down**.

```
Fear 80% → Can't think strategically
Rage 85% → Can't consider consequences  
Grief 90% → Can't form complex thoughts

Available actions: Reflexive, instinctive, pattern-based only
```

**Example**:
```
❌ FAKE:
"陈锋极度恐惧,但他冷静地分析了逃跑路线。"

(Impossible. Can't be "极度恐惧" and "冷静分析" simultaneously.
Pick one: Either fear is moderate, or analysis is impossible)

✅ REAL:
"陈锋在跑。不知道往哪跑。就是跑。"

(Fear 95% = no planning, pure motor execution)
```

---

## §5 Perception Is Filtered
## §5 感知是被过滤的

### 5.1 Attention Is A Spotlight

**Human can only consciously process ~1 thing at a time**.

```
focus_target = "monster"

Environment contains:
- Monster (IN FOCUS - high detail)
- Trees (peripheral - low detail, just "brown blur")  
- Sky color (NOT PROCESSED - attention not allocated)
- Ground texture (NOT PROCESSED)
- Distant sounds (NOT PROCESSED)
```

**Writing rule**: 

When character is focused on X, they CANNOT simultaneously notice Y in detail.

```
❌ WRONG:
"陈锋盯着妖兽，同时注意到队友脸上的表情，
还看到了天空的颜色异常。"

(Brain doesn't work like this. Focus = tunnel vision)

✅ RIGHT:
"陈锋盯着妖兽。有人在喊——谁？——没时间转头。"

(Hearing detected, but can't process content while maintaining visual focus)
```

---

### 5.2 Memory Is Unreliable

**Under stress, memory encoding fails**.

```
stress_level > 70:
└─ Short-term memory impaired
    ├─ Events blur together  
    ├─ Time estimation broken
    └─ Details lost

stress_level > 90:
└─ Memory fragmentation
    └─ Later recall: "I remember X... then... suddenly Y"
        (Missing the middle - brain never recorded it)
```

**Writing implication**:

If character experienced high-stress event, they CANNOT have perfect recall:

```
✅ REAL:
"陈锋试图回想——李工是怎么死的？剑光？
还是枪声？还是先看到剑？特么的，记不清了。"

(This is normal. Traumatic memories are fragmented)
```

---

### 5.3 Sensory Overload Causes Dropout

**When too much is happening, brain drops inputs**.

```
sensory_overload > 80:
├─ Visual: Sees movement but not details
├─ Auditory: Hears noise but can't distinguish sources  
├─ Tactile: Numbness or hypersensitivity (random)
└─ Integration: Can't combine senses (see explosion, don't hear it)
```

**Example**:
```
✅ REAL:
"爆炸——陈锋看到火光，但没听到声音。
耳朵在响。是耳鸣！草,听不见了。"

(Sensory dropout during overload. Realistic)
```

---

## §6 Dialogue = Incomplete Thoughts
## §6 对话 = 不完整的想法

### 6.1 People Don't Speak In Essays

**Humans speak in fragments, not finished thoughts**.

```
❌ FAKE DIALOGUE:
"我认为我们应该撤退,因为敌人太强,
而且我们的弹药不足,继续战斗是不理智的。"

(No human talks like this under stress)

✅ REAL DIALOGUE:
"撤！"
"弹药——"  
"草,撤！"

(Incomplete, interrupted, urgent)
```

---

### 6.2 Subtext > Text

**Most important communication is non-verbal**.

```
✅ REAL:
"没事。"他转身就走。

(Says "没事", body says "我不想谈". Reader gets both messages)

❌ FAKE:
"没事。"他说,但语气里充满了愤怒。

(Don't label the subtext. Show it through action)
```

---

### 6.3 People Interrupt & Overlap

```
✅ REAL:
"我们得——"
"没时间了！"
"听我说——"  
"跑！"

❌ FAKE:
"我们得想办法。"
"是的,但现在没时间了。"
"那我们——"
"快跑！"

(Too polite. Too turn-taking. Not how panicked humans talk)
```

---

## §7 Scene Writing: Simulation Mode
## §7 场景写作：模拟模式

### 7.1 Before Writing: Boot Character

```
INITIALIZATION:
1. Load character knowledge base
2. Set physical state (injuries, exhaustion, pain)
3. Set stress level → determine available brain layers
4. Set focus target (what they're paying attention to)
5. Load immediate environment (only what they can perceive)

NOW: Simulate forward.
```

---

### 7.2 Writing = Reporting Simulation

**You are not creating actions. You are observing them**.

```
PROCESS:
1. Input arrives (stimulus)
2. Character's current state processes it
3. Output emerges (action/reaction)  
4. Write what happened

NOT:
1. Plot needs character to do X
2. Make character do X
3. Justify why they did X
```

---

### 7.3 Simulation Rules

**Rule 1: Character can only respond to inputs they received**

```
Available inputs:
✅ What they see (in current focus cone)
✅ What they hear (if attention available)
✅ What they feel (body sensations)
✅ What they remember (if recall successful)

NOT available:
❌ What author knows
❌ What other characters are thinking
❌ Information not yet revealed
```

**Rule 2: Response emerges from current state**

```
stress=90, pain=8, exhaustion=75, focus="escape"

Input: "队友在喊他的名字"
Processing: 
- Auditory input detected
- But focus locked on escape route
- Brain categorizes voice as "background noise"
- No response generated

Output: 陈锋继续跑，没听到。

(Not ignoring teammate. Brain literally didn't process the input)
```

**Rule 3: Let simulation surprise you**

```
If you planned: "Character calmly explains the situation"

But simulation shows: stress=85, available_layers=[1,2,3]

Then: Character CANNOT calmly explain. Layer 4 (problem-solving) 
is offline. Character can only give fragmented reactions.

→ Throw out your plan. Write what simulation shows.
```

---

## §8 Anti-Patterns: What Triggers Fakeness
## §8 反模式：什么会触发假感

### 8.1 The Omniscient Character

```
❌ TRIGGERS:
"他意识到这是陷阱。"
"她明白了对方的意图。"  
"他发现了隐藏的线索。"

WHY FAKE: Character suddenly has author's god-view.

FIX: Show the actual thought process:
"等等——为什么门开着？"
"他刚才看我的眼神... 草，他早就知道了。"
"墙上有刮痕。新的。有人来过。"
```

---

### 8.2 The Emotion Label

```
❌ TRIGGERS:
"他感到X"
"她充满了Y"
"他的心情是Z"

WHY FAKE: Emotions are body states, not labels.

FIX: Show physiological response:
"他的手在抖。"
"她的视线模糊了。"
"他的呼吸急促起来。"
```

---

### 8.3 The Convenient Realization

```
❌ TRIGGERS:
"突然,他想到了一个办法。"
"这时她发现了关键信息。"

WHY FAKE: Plot-serving timing. Brain doesn't work on plot schedule.

FIX: Show trigger → recognition:
"他的手碰到口袋——打火机——等等——"
"那个符号——她在哪见过？——对了，昨天的报告——"
```

---

### 8.4 The Painless Injury

```
❌ TRIGGERS:
"他的腿断了,但继续战斗。"
"她忍着伤痛,保持冷静。"

WHY FAKE: Pain doesn't pause for heroism.

FIX: Pain intrudes constantly:
"他挥拳——腿传来撕裂般的痛——草——拳头偏了。"
"她深呼吸,试图集中——伤口在烧——特么的——集中不了。"
```

---

### 8.5 The Literary Observation

```
❌ TRIGGERS:  
"天空像..."
"空气中弥漫着某种..."
"仿佛..."

WHY FAKE: Character became a poet. Brain in crisis mode doesn't generate metaphors.

FIX: Raw perception:
"天空是紫色的。"
"很不对劲。"
```

---

## §9 Quality Check: The Human Test
## §9 质量检查：人类测试

### 9.1 After Writing Each Scene

Run these tests:

#### Test 1: Information Audit
```
For each fact character "knows":
- How did they learn it?  
- Did they have attention available when info appeared?
- Did their brain have capacity to process it?

If you can't answer → Character doesn't actually know it → Rewrite
```

#### Test 2: Body Continuity
```
List all injuries/exhaustion at scene start.
Check: Is EVERY action affected by this state?

If any action ignores body state → Rewrite
```

#### Test 3: Stress-Layer Match
```
Character's stress level = ?
Available brain layers = ?  
Character's actions in scene = ?

Do actions require layers that are offline?
If yes → Rewrite (character can't do that)
```

#### Test 4: The Surprise Test
```
Did character do something that surprised you while writing?

If yes → Good sign (simulation running)
If no → Bad sign (you're puppeting them)
```

---

### 9.2 The Ultimate Test: Be The Character

```
Close eyes.
Imagine you are the character.
You have their injuries, exhaustion, fear, confusion.
You know ONLY what they know.

Now: Would you actually do what they did in the scene?
Or would you be too scared/tired/confused?

If honest answer is "I'd probably just freeze" 
→ That's what character should do
```

---

## §10 Execution Checklist
## §10 执行清单

### Before Writing

- [ ] Initialize character state (physical, cognitive, emotional)
- [ ] Determine stress level → available brain layers
- [ ] List what character knows vs what author knows
- [ ] Set focus target (what has their attention)
- [ ] Load only perceivable environment

### During Writing  

- [ ] Every action: Check if character's state allows it
- [ ] Every realization: Show trigger → thought process
- [ ] Every emotion: Write body response, not label
- [ ] Every injury: Affects ALL subsequent actions
- [ ] Every input: Filter through attention/stress

### After Writing

- [ ] Information audit (how did they learn each fact?)
- [ ] Body continuity check (injuries still matter?)
- [ ] Stress-layer match (actions match available cognition?)
- [ ] Zero emotion labels? (all shown through body?)
- [ ] Character surprised you at least once?

---

## §11 Special Case: The First Death Scene
## §11 特殊案例：第一次目击死亡

Since CH1 involves watching teammates die, here's the human response model:

### 11.1 First-Time Death Witness Response

**Stage 1: Cognitive Rejection (0-3 seconds)**
```
Brain refuses to process what eyes see.
"那是特效。假的。"
Denial is automatic, not a choice.
```

**Stage 2: Physiological Cascade (3-10 seconds)**
```
Body overrides brain:
- Adrenaline dump (time dilation, tunnel vision)
- Nausea (vomit reflex activates)
- Legs weaken (blood redirects to core)
- Breathing stops (freeze response)
```

**Stage 3: System Crash (10-30 seconds)**
```
If stress > 95:
- Dissociation (feels unreal, watching from outside body)
- Auditory exclusion (sound cuts out)
- Memory fragmentation (won't remember clearly later)

Character is NOT in control. Autopilot takes over.
```

**Stage 4: Survival Override (30+ seconds)**
```
Hindbrain takes control:
- Run (if escape route visible)
- Freeze (if no clear escape)
- Fight (if cornered)

No planning. Pure reflex.
```

---

### 11.2 Writing Rules For Death Scenes

```
❌ NEVER:
"他看到队友死了,悲痛欲绝,但强迫自己继续任务。"

(Impossible sequence. Can't jump from "悲痛欲绝" to "理性执行任务")

✅ ALWAYS:
Show the stages:
1. Denial: "不是真的"
2. Body crash: (vomiting, shaking)
3. Dissociation: "这不是在发生"  
4. Autopilot: (running, no thoughts)

Grief comes LATER (hours/days), not immediately.
```

---

## §12 Final Principle: Trust The Simulation
## §12 最终原则：相信模拟

**When simulation and plot conflict**:

```
IF: Plot needs character to do X
BUT: Simulation shows character would do Y (because of their state)

THEN: Let character do Y.

Change the plot. Don't break the character.
```

**Why?**

Readers have human brains. They run unconscious simulations of characters. When your character breaks human rules, reader's simulation errors out → "fake feeling".

Your job: Make the simulation run smoothly in reader's brain.

**How?**

Stop writing what you want to happen.
Start writing what would actually happen.

---

**END OF SOP v6.0**

