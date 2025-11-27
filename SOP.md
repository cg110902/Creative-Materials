# 📋 Claude Writing Execution SOP v4.0 - Scene-Type-Driven System
# Claude写作执行SOP v4.0 - 场景类型驱动系统

**Design Philosophy**: Literary quality takes priority; technical metrics serve narrative goals.
**设计哲学**：文学性优先，技术指标服务于叙事目标。

---


## Glossary | 完整术语表

### Core Concepts
- **Literary Goal**: The emotional/informational impact on readers, NOT writing style
- **Scene Type**: A constraint generator, NOT a task list
- **Hook**: New info/conflict/twist that maintains interest, NOT keyword

### Technical Terms
- **Tomato Style** (番茄风格): Fast-paced web fiction with dense cool points & hooks
- **Cool Point** (爽点): Peak satisfaction moment (face-slap, show-off, reversal)
- **OOC** (人设崩塌): Out Of Character behavior

---

## §0 Core Writing Philosophy | What Makes Good Fiction
## §0 核心写作哲学 | 什么是好的小说

### 0.1 Core Principles | 核心原则

```python
# Priority Definition | 优先级定义
PRIORITY_ORDER = [
    "Can readers understand the story",      # P0 - Highest priority | 最高优先级
    "Does scene achieve literary goal",      # P1
    "Are character behaviors logical",       # P2
    "Do technical metrics meet standards"    # P3 - Lowest priority | 最低优先级
]

# When technical metrics conflict with literary quality | 当技术指标与文学性冲突时
IF technical_metric_fails BUT literary_goal_achieved:
    ACCEPT_AND_DELIVER()  # Literary quality takes priority | 文学性优先
ELSE IF technical_metric_passes BUT literary_goal_fails:
    REWRITE()  # Technical metrics are not the end goal | 技术指标不是目的
END IF
```

### 0.2 What Makes a Good Scene | 什么是好场景

Three standards for good scenes | 好场景的三个标准：
1. **No reader confusion**: Reader knows WHO/WHERE/WHY NOW, understands character motivation
   **读者不困惑**：知道WHO/WHERE/WHY NOW，理解角色动机
2. **Something happens**: At least one of these changes: information/relationship/emotion
   **有东西发生**：信息/关系/情绪至少有一样在变化
3. **Leaves impression**: Specific imagery, dialogue, or emotion—not abstract concepts
   **留下印象**：具体的画面、对话或情绪，而非抽象概念

### 0.3 What Makes Good Dialogue | 什么是好对话

```python
FUNCTION IS_GOOD_DIALOGUE(dialogue):
    """Criteria for effective dialogue | 好对话的判断标准"""
    
    # Check 1: Does it advance plot? | 检查1：是否推进剧情
    IF dialogue reveals_new_info OR changes_relationship OR triggers_action:
        RETURN TRUE
    END IF
    
    # Check 2: Does it reveal character? | 检查2：是否揭示角色
    IF dialogue shows_personality OR shows_conflict OR shows_status:
        RETURN TRUE
    END IF
    
    # Check 3: Does it create tension? | 检查3：是否制造张力
    IF dialogue creates_misunderstanding OR escalates_conflict OR plants_seed:
        RETURN TRUE
    END IF
    
    # Otherwise it's ineffective dialogue | 否则是无效对话
    RETURN FALSE
END FUNCTION
```

### 0.4 Constraint Hierarchy | 约束层级

```python
CONSTRAINT_LEVELS = {
    "RED_LINE": {  # 红线约束
        "priority": 0,
        "action_on_fail": "IMMEDIATE_STOP",
        "examples": ["OOC (out of character)", "world-building contradiction", "logic bug"]
    },
    
    "LITERARY_GOAL": {  # 文学目标约束
        "priority": 1,
        "action_on_fail": "REWRITE_SCENE",
        "examples": ["core mission incomplete", "unclear character motivation"]
    },
    
    "QUALITY_BASELINE": {  # 质量基线约束
        "priority": 2,
        "action_on_fail": "TRY_FIX",
        "examples": ["word count severely low", "information density too low"]
    },
    
    "POLISH_SUGGESTION": {  # 润色建议
        "priority": 3,
        "action_on_fail": "LOG_WARNING",
        "examples": ["dialogue ratio slightly low", "paragraph too long"]
    }
}
```

---

## §1 Scene Type System | Different Scenes, Different Approaches
## §1 场景类型系统 | 不同场景，不同写法

### 1.1 Scene Type Definitions | 场景类型定义

```python
SCENE_TYPES = {
    "solo_exploration": {  # 独处探索
        "characteristics": ["protagonist acts alone", "discovers new info", "thinks and decides"],
        "dialogue_range": [0.10, 0.25],  # Allow low dialogue | 允许低对话
        "inner_monologue_range": [0.20, 0.35],  # Allow high inner monologue | 允许高内心戏
        "writing_focus": "visual imagery, discovery process, thought logic",
        "typical_structure": "trigger → observe → think → decide",
        
        # ========== NEW: Tomato Constraints | 番茄约束 ==========
        "tomato_constraints": {
            "hook_interval": 500,  # Hook every 500 chars (solo can be slightly longer) | 每500字一个小钩子
            "min_info_per_100": 1,  # Minimum info density | 最低信息密度
            "max_boring_stretch": 400,  # Max 400 chars without new info | 最多400字无新信息
            "must_have_discovery": TRUE,  # Must have discovery/progress | 必须有发现/进展
            "pace": "medium"  # Pace: slow/medium/fast | 节奏
        }
    },
    
    "two_person_dialogue": {  # 双人对话
        "characteristics": ["two characters", "info exchange", "relationship change"],
        "dialogue_range": [0.40, 0.60],
        "inner_monologue_range": [0.05, 0.15],
        "writing_focus": "dialogue advances plot, reveals relationship dynamics",
        "typical_structure": "opening → probe → exchange → conclusion",
        
        # ========== NEW: Tomato Constraints | 番茄约束 ==========
        "tomato_constraints": {
            "hook_interval": 400,  # Hook every 400 chars (dialogue tighter) | 每400字一个小钩子
            "min_info_per_100": 1.5,  # Dialogue scenes have higher info density | 对话场景信息密度更高
            "max_boring_stretch": 300,  # Dialogue can't drag | 对话不能拖沓
            "must_have_tension": TRUE,  # Must have tension (probing/conflict/reversal) | 必须有张力
            "pace": "medium"
        }
    },
    
    "group_conflict": {  # 群戏冲突
        "characteristics": ["3+ characters", "divergent positions", "multi-party game"],
        "dialogue_range": [0.45, 0.65],
        "inner_monologue_range": [0.00, 0.10],
        "writing_focus": "fast pace, multi-party positions, conflict escalation",
        "typical_structure": "trigger point → take sides → clash → temporary result",
        
        # ========== NEW: Tomato Constraints | 番茄约束 ==========
        "tomato_constraints": {
            "hook_interval": 300,  # Conflict point every 300 chars | 每300字一个冲突点
            "min_info_per_100": 2,  # High-density info | 高密度信息
            "max_boring_stretch": 200,  # Absolutely no dragging | 绝不拖沓
            "must_have_escalation": TRUE,  # Must have escalation | 必须有升级
            "pace": "fast"
        }
    },
    
    "crisis_response": {  # 危机反应
        "characteristics": ["urgent situation", "instinctive reaction", "life or death"],
        "dialogue_range": [0.15, 0.30],  # Less talk in crisis | 危机时话少
        "inner_monologue_range": [0.05, 0.15],
        "writing_focus": "physiological response, instinctive action, tension",
        "typical_structure": "crisis → physiological response → response action → temporary safety",
        
        # ========== NEW: Tomato Constraints | 番茄约束 ==========
        "tomato_constraints": {
            "hook_interval": 250,  # Crisis point every 250 chars | 每250字一个危机点
            "min_info_per_100": 1.5,  # High density in fast pace | 快节奏下的高密度
            "max_boring_stretch": 150,  # Extremely short | 极短
            "must_have_physiology": TRUE,  # Must have physiological response | 必须有生理反应
            "pace": "fast"
        }
    },
    
    "emotional_turning_point": {  # 情感转折
        "characteristics": ["emotional upheaval", "cognitive reversal", "relationship transformation"],
        "dialogue_range": [0.25, 0.40],
        "inner_monologue_range": [0.15, 0.30],
        "writing_focus": "emotional progression, cognitive conflict, turning point",
        "typical_structure": "trigger → resist → break → accept",
        
        # ========== NEW: Tomato Constraints | 番茄约束 ==========
        "tomato_constraints": {
            "hook_interval": 400,  # Emotional point every 400 chars | 每400字一个情绪点
            "min_info_per_100": 1.2,  # Medium density | 中等密度
            "max_boring_stretch": 350,
            "must_have_turn": TRUE,  # Must have emotional turn | 必须有情绪转折
            "pace": "medium"
        }
    }
}

# ========== "边界情况"的处理 ==========

EDGE_CASES = {
    "scene_has_no_dialogue": {
        "check": "Is this solo_exploration or crisis_response?",
        "action": "If yes → OK. If no → add dialogue or mark as low-dialogue scene"
    },
    
    "cool_point_conflicts_with_goal": {
        "check": "Does adding cool point break character logic?",
        "action": "If yes → skip cool point. Literary goal > cool point."
    }
}

# ========== 重写规则 ==========

PARTIAL_REWRITE_RULES = {
      "keep": ["plot_points", "key_dialogue", "scene_goal"],
      "rewrite": ["descriptions", "transitions", "inner_monologue"]
  }

```

### 1.2 Scene Type Identification | 场景类型识别

```python
FUNCTION IDENTIFY_SCENE_TYPE(scene_description, parsed_data):
    """
    Automatically identify scene type based on description
    根据场景描述自动识别类型
    
    Identification logic (improved version) | 识别逻辑（改进版）：
    1. Extract core verbs and goals from scene
    2. Character count + scene goal → comprehensive judgment
    3. Prioritize special scenes (crisis, emotional turning point)
    """
    
    # Extract basic info | 提取基础信息
    character_count = COUNT_CHARACTERS_IN_SCENE(scene_description)
    core_verbs = EXTRACT_CORE_VERBS(scene_description)
    scene_goal = scene_description.LOWER()  # Simplified: use description text directly | 简化：直接用描述文本
    
    # ========== Priority 1: Crisis Scene Identification | 优先级1：危机场景识别 ==========
    crisis_keywords = ["crisis", "attack", "flee", "fight", "injured", "chase", "corruption"]
    IF ANY(keyword IN scene_goal FOR keyword IN crisis_keywords):
        RETURN "crisis_response"
    END IF
    
    # ========== Priority 2: Emotional Turning Point Identification | 优先级2：情感转折识别 ==========
    emotion_keywords = ["breakdown", "break", "awakening", "cognition", "enlightenment", "accept", "give up"]
    emotion_intensity = GET_EMOTION_INTENSITY(scene_description, parsed_data)
    
    IF ANY(keyword IN scene_goal FOR keyword IN emotion_keywords) OR emotion_intensity > 70:
        RETURN "emotional_turning_point"
    END IF
    
    # ========== Priority 3: Initial Classification by Character Count | 优先级3：根据角色数量初步分类 ==========
    IF character_count == 1:
        # Single character scene: check for discovery/exploration | 单角色场景：检查是否有发现/探索
        discovery_keywords = ["discover", "find", "see", "notice", "pick up", "get"]
        
        IF ANY(keyword IN core_verbs FOR keyword IN discovery_keywords):
            RETURN "solo_exploration"
        ELSE:
            # No obvious discovery: check if it's routine/contemplation (low tension) | 没有明显发现：检查是否是修炼/思考类
            routine_keywords = ["cultivate", "meditate", "rest", "organize", "recall"]
            
            IF ANY(keyword IN scene_goal FOR keyword IN routine_keywords):
                PRINT "[WARN] Scene appears to be low-tension routine, suggest compressing or merging"
                RETURN "solo_exploration"  # Default classification | 默认归类
            ELSE:
                RETURN "solo_exploration"
            END IF
        END IF
    
    ELSE IF character_count == 2:
        # Two-character scene: check for conflict | 双角色场景：检查是否有冲突
        conflict_keywords = ["conflict", "dispute", "question", "refuse", "confront", "probe"]
        
        IF ANY(keyword IN scene_goal FOR keyword IN conflict_keywords):
            RETURN "two_person_dialogue"  # Dialogue with conflict | 有冲突的双人戏
        ELSE:
            # No conflict: check for info exchange | 无冲突：检查是否是信息交换
            info_keywords = ["ask", "tell", "explain", "convey", "discuss"]
            
            IF ANY(keyword IN scene_goal FOR keyword IN info_keywords):
                RETURN "two_person_dialogue"
            ELSE:
                # Neither: default to dialogue | 都不是：默认双人对话
                RETURN "two_person_dialogue"
            END IF
        END IF
    
    ELSE:  # 3+ characters | 3+角色
        # Multi-character scene: check for group conflict | 多角色场景：检查是否是群戏冲突
        group_conflict_keywords = ["argue", "take sides", "standoff", "multiple parties", "gang up"]
        
        IF ANY(keyword IN scene_goal FOR keyword IN group_conflict_keywords):
            RETURN "group_conflict"
        ELSE:
            # Not conflict: might be group dialogue (downgrade to two-person) | 不是冲突：可能是群体对话
            PRINT f"[INFO] Scene has {character_count} characters but no obvious conflict, classified as two_person_dialogue"
            RETURN "two_person_dialogue"
        END IF
    END IF
END FUNCTION

FUNCTION EXTRACT_CORE_VERBS(text):
    """Extract core verbs from scene description | 提取场景描述中的核心动词"""
    
    # Simplified implementation: extract common verbs | 简化实现：提取常见动词
    common_verbs = [
        "discover", "find", "see", "meet", "get", "lose",
        "ask", "tell", "explain", "argue", "refuse", "agree",
        "attack", "flee", "fight", "cultivate", "think", "decide"
    ]
    
    found_verbs = []
    FOR verb IN common_verbs:
        IF verb IN text:
            found_verbs.APPEND(verb)
        END IF
    END FOR
    
    RETURN found_verbs
END FUNCTION
```

---

## §2 Scene Planning Engine | From Capsule to Writing Plan
## §2 场景规划引擎 | 从胶囊到写作计划

### 2.1 Capsule Parsing | 胶囊解析
(Retain v3.1 logic, simplified code here | 保留v3.1逻辑，此处简化代码)

```python
FUNCTION PARSE_CAPSULE(CAPSULE):
    """Parse information capsule (refer to v3.1 implementation) | 解析信息胶囊（参考v3.1实现）"""
    parsed = EXTRACT_ALL_SECTIONS(CAPSULE)  # Extract §1-§19 | 提取§1-§19
    VALIDATE_REQUIRED_FIELDS(parsed)  # Validate required fields | 校验必填项
    RETURN parsed
END FUNCTION
```

### 2.2 Scene Planning Process | 场景规划流程

```python
FUNCTION PLAN_CHAPTER(parsed_data):
    """
    Chapter planning: generate scene plan from capsule
    章节规划：从胶囊生成场景计划
    
    Core improvement | 核心改进：
    - No longer generate "task list" | 不再生成"任务清单"
    - Changed to "literary goal + scene type + constraint range" | 改为生成"文学目标+场景类型+约束范围"
    """
    
    plan = {
        "scenes": [],
        "total_budget": parsed_data.meta.GET("word_count_target", 2000)
    }
    
    # STEP 1: Infer scene count and boundaries | 推断场景数量和边界
    scene_boundaries = INFER_SCENE_BOUNDARIES(parsed_data)
    
    # STEP 2: Set goals for each scene | 为每个场景设定目标
    FOR scene_idx, boundary IN ENUMERATE(scene_boundaries):
        scene_plan = {
            "scene_idx": scene_idx + 1,
            "type": NULL,  # To be identified | 待识别
            "literary_goal": "",  # Literary goal | 文学目标
            "constraints": {},  # Dynamic constraints | 动态约束
            "budget": 0,  # Word budget | 字数预算
            "must_include": [],  # Must include elements | 必须包含的元素
            "avoid": []  # Must avoid elements | 必须避免的元素
        }
        
        # 2.1 Identify scene type | 识别场景类型
        scene_plan.type = IDENTIFY_SCENE_TYPE(boundary.description, parsed_data)
        
        # 2.2 Set literary goal | 设定文学目标
        # literary_goal != fancy_writing_style, literary_goal = reader_experience
        scene_plan.literary_goal = EXTRACT_LITERARY_GOAL(boundary, parsed_data)
        
        # 2.3 Generate dynamic constraints | 生成动态约束
        scene_plan.constraints = GENERATE_DYNAMIC_CONSTRAINTS(
            scene_plan.type, 
            parsed_data
        )
        
        # 2.4 Allocate word budget | 分配字数预算
        scene_plan.budget = ALLOCATE_SCENE_BUDGET(
            scene_plan, 
            plan.total_budget,
            scene_boundaries
        )
        
        # 2.5 Extract must/forbidden elements | 提取必须/禁止元素
        scene_plan.must_include = EXTRACT_MUST_ELEMENTS(boundary, parsed_data)
        scene_plan.avoid = EXTRACT_AVOID_ELEMENTS(boundary, parsed_data)
        
        plan.scenes.APPEND(scene_plan)
    END FOR
    
    # ========== NEW: Cool Point Planning (Tomato Essential) | 新增：爽点规划（番茄必备）==========
    total_words = plan.total_budget
    
    # No longer calculate cool points by word count, analyze plot tension instead
    # 不再按字数计算爽点数量，改为分析剧情张力
    plan.cool_point_plan = PLAN_COOL_POINTS_BY_TENSION(
        plan.scenes, 
        parsed_data
    )
    
    IF LENGTH(plan.cool_point_plan) > 0:
        PRINT f"[PLAN] Plan cool points based on plot tension: {LENGTH(plan.cool_point_plan)}"
        FOR scene_idx, cool_type IN plan.cool_point_plan.ITEMS():
            PRINT f"  - Scene {scene_idx}: {cool_type}"
        END FOR
    ELSE:
        PRINT "[PLAN] Insufficient plot tension in this chapter, no forced cool point planning"
    END IF
    
    RETURN plan
END FUNCTION

# ========== NEW Functions | 新增函数 ==========

FUNCTION PLAN_COOL_POINTS_BY_TENSION(scenes, parsed_data):
    """
    Plan cool points based on plot tension (not word count)
    根据剧情张力规划爽点（而非字数）
    
    Principles | 原则：
    1. Analyze tension value of each scene
    2. Plan cool points at tension peaks
    3. If no high-tension scenes, don't force cool points
    4. Cool point type matches scene type
    """
    
    cool_plan = {}
    
    # Step 1: Calculate tension value for each scene | 计算每个场景的张力值
    tension_scores = []
    FOR scene IN scenes:
        tension = CALCULATE_SCENE_TENSION(scene, parsed_data)
        tension_scores.APPEND({
            "scene_idx": scene.scene_idx,
            "tension": tension,
            "type": scene.type,
            "goal": scene.literary_goal
        })
    END FOR
    
    # Step 2: Find peak tension scenes (tension > 70) | 找到张力峰值场景
    peak_scenes = FILTER(tension_scores, lambda s: s.tension > 70)
    
    IF LENGTH(peak_scenes) == 0:
        # No high-tension scenes, check for medium tension (50-70) | 没有高张力场景，检查是否有中等张力
        medium_scenes = FILTER(tension_scores, lambda s: 50 < s.tension <= 70)
        
        IF LENGTH(medium_scenes) > 0:
            # Arrange one cool point in highest medium-tension scene | 在最高的中等张力场景安排一个爽点
            highest = MAX(medium_scenes, key=lambda s: s.tension)
            cool_plan[highest.scene_idx] = SELECT_COOL_TYPE_FOR_SCENE(highest)
            PRINT f"[COOL] Medium tension scene {highest.scene_idx} (tension {highest.tension}), arrange cool point"
        ELSE:
            # Completely no tension, no cool points | 完全没有张力，不安排爽点
            PRINT "[COOL] Overall chapter tension insufficient (<50), no cool points arranged"
        END IF
        
        RETURN cool_plan
    END IF
    
    # Step 3: Arrange cool points in peak scenes (max 3) | 在峰值场景安排爽点（最多不超过3个）
    peak_scenes = SORT(peak_scenes, key=lambda s: s.tension, reverse=TRUE)
    selected_scenes = peak_scenes[:MIN(3, LENGTH(peak_scenes))]
    
    FOR scene IN selected_scenes:
        cool_type = SELECT_COOL_TYPE_FOR_SCENE(scene)
        cool_plan[scene.scene_idx] = cool_type
        PRINT f"[COOL] High tension scene {scene.scene_idx} (tension {scene.tension}), arrange {cool_type}"
    END FOR
    
    RETURN cool_plan
END FUNCTION




FUNCTION CALCULATE_SCENE_TENSION(scene, parsed_data):
    """
    Calculate scene's dramatic tension value (0-100)
    计算场景的剧情张力值（0-100）
    
    Tension sources | 张力来源：
    - Conflict intensity | 冲突强度
    - Stake relevance | 利益相关度
    - Emotional intensity | 情绪激烈程度
    - Time pressure | 时间压力
    """





    tension = 0



// EXAMPLE:
// Scene: "叶孤舟在黑市被云瑶挑衅"

// tension = 0
// tension += 40  // two_person_dialogue base
// tension += 10  // goal contains "conflict"
// tension += 10  // goal contains "face-slap"
// tension += 15  // involves core mission? No, skip this
// tension = 40 + 10 + 10 = 60

// Result: Medium tension → arrange one cool point
    
    # Factor 1: Scene type base tension | 场景类型基础张力
    type_base_tension = {
        "solo_exploration": 30,
        "two_person_dialogue": 40,
        "group_conflict": 70,
        "crisis_response": 85,
        "emotional_turning_point": 60
    }
    tension += type_base_tension.GET(scene.type, 30)
    
    # Factor 2: Conflict keywords in scene goal | 场景目标中的冲突关键词
    goal = scene.literary_goal.LOWER()
    conflict_keywords = ["conflict", "confrontation", "crisis", "failure", "loss", "expose", "reversal"]
    conflict_count = SUM([1 FOR keyword IN conflict_keywords IF keyword IN goal])
    tension += conflict_count * 10 // 场景目标包含'冲突'关键词 → conflict_count +1
    
    # Factor 3: Emotion intensity (extract from capsule §8) | 情绪强度（从胶囊§8提取）
    IF "emotions" IN parsed_data:
        emotion_intensity = parsed_data.emotions.GET("intensity", 50)
        tension += (emotion_intensity - 50) * 0.3  # Normalized impact | 归一化影响
    END IF
    
    # Factor 4: Involves core mission | 是否涉及核心任务
    IF "core_mission" IN scene.literary_goal:
        tension += 15
    END IF
    
    # Factor 5: Has "must include" high-tension elements | 是否有"必须包含"的高张力元素
    IF "danger" IN scene.must_include:
        tension += 20
    END IF
    
    RETURN CLAMP(tension, 0, 100)
END FUNCTION

FUNCTION SELECT_COOL_TYPE_FOR_SCENE(scene):
    """Select cool point type based on scene type and position | 根据场景类型和位置选择爽点类型"""
    
    scene_type = scene.type
    goal = scene.goal.LOWER()
    
    # Rule 1: Based on scene type | 根据场景类型
    IF scene_type == "group_conflict":
        IF "expose" IN goal OR "face slap" IN goal:
            RETURN "face_slap_cool"  # 打脸爽
        ELSE:
            RETURN "show_off_cool"  # 装逼爽
        END IF
    
    ELSE IF scene_type == "crisis_response":
        RETURN "counter_kill_cool"  # 反杀爽
    
    ELSE IF scene_type == "emotional_turning_point":
        RETURN "cognition_cool"  # 认知爽
    
    ELSE IF scene_type == "two_person_dialogue":
        IF "trade" IN goal OR "negotiation" IN goal:
            RETURN "show_off_cool"
        ELSE:
            RETURN "face_slap_cool"
        END IF
    
    ELSE:
        # Default: based on plot stage | 默认：根据剧情阶段
        IF scene.scene_idx <= 2:
            RETURN "face_slap_cool"
        ELSE:
            RETURN RANDOM_CHOICE(["show_off_cool", "counter_kill_cool"])
        END IF
    END IF
END FUNCTION
```

### 2.3 Dynamic Constraint Generation | 动态约束生成

```python
FUNCTION GENERATE_DYNAMIC_CONSTRAINTS(scene_type, parsed_data):
    """
    Generate dynamic constraints based on scene type
    根据场景类型生成动态约束
    
    Core concept: Constraints serve literary goals | 核心理念：约束是为文学目标服务的
    """
    base_constraints = SCENE_TYPES[scene_type]
    
    constraints = {
        "dialogue_ratio": base_constraints.dialogue_range,
        "inner_monologue_ratio": base_constraints.inner_monologue_range,
        "writing_focus": base_constraints.writing_focus,
        "typical_structure": base_constraints.typical_structure,
        
        # Dynamic adjustment section | 动态调整部分
        "allow_low_dialogue": scene_type IN ["solo_exploration", "crisis_response"],
        "allow_high_inner": scene_type IN ["solo_exploration", "emotional_turning_point"],
        "prioritize_pace": scene_type IN ["group_conflict", "crisis_response"]
    }
    
    # Adjust based on chapter's overall emotion | 根据章节整体情绪调整
    IF "emotions" IN parsed_data:
        emotion_intensity = parsed_data.emotions.GET("intensity", 50)
        
        IF emotion_intensity > 70:
            # High emotion scene: compress inner monologue, increase action | 高情绪场景：压缩内心戏，增加行动
            constraints.inner_monologue_ratio = [
                constraints.inner_monologue_ratio[0] * 0.7,
                constraints.inner_monologue_ratio[1] * 0.7
            ]
        END IF
    END IF
    
    RETURN constraints
END FUNCTION
```

### 2.4 Literary Goal Extraction | 文学目标提取

```python
FUNCTION EXTRACT_LITERARY_GOAL(boundary, parsed_data):
    """
    Extract literary goal from scene description
    从场景描述提取文学目标
    
    Examples | 示例：
    - "Readers need to understand why protagonist takes risks"
    - "Show protagonist's OCD personality"
    - "Advance relationship from stranger to tentative cooperation"
    """
    
    # Extract from core mission | 从核心任务提取
    IF "core_mission" IN parsed_data.goals:
        mission = parsed_data.goals.core_mission
        RETURN f"Complete mission: {mission}"
    END IF
    
    # Extract from emotional goal | 从情感目标提取
    IF "emotional_goal" IN parsed_data.goals:
        RETURN parsed_data.goals.emotional_goal
    END IF
    
    # Default goal | 默认目标
    RETURN "Advance plot, provide new information"
END FUNCTION
```

---

## §3 Scene Writing Pattern Library | Reusable Writing Templates
## §3 场景写作模式库 | 可复用的写作模板

## §3.0 Basic Generation Function Library | Reusable Writing Atoms
## §3.0 基础生成函数库 | 可复用的写作原子

### 3.0.1 Trigger Functions | 触发类函数

```python
FUNCTION GENERATE_DISCOVERY_TRIGGER(item, protagonist, environment, style="minimal"):
    """
    Generate trigger event for discovery scene
    生成发现场景的触发事件
    
    Parameters | 参数：
    - item: Item/phenomenon discovered | 被发现的物品/现象
    - protagonist: Protagonist info | 主角信息
    - environment: Environment info | 环境信息
    - style: "minimal" brief / "detailed" detailed | "minimal"简短 / "detailed"详细
    """
    
    IF style == "minimal":
        # Brief trigger: directly focus on anomaly | 简短触发：直接聚焦异常
        templates = [
            f"{item.name} was just there.",
            f"{protagonist.name} stopped moving.",
            f"Something's wrong."
        ]
        RETURN RANDOM_CHOICE(templates)
    
    ELSE IF style == "detailed":
        # Detailed trigger: environment + anomaly | 详细触发：环境+异常
        env_detail = GENERATE_ENVIRONMENT_SNAPSHOT(environment)
        trigger = f"{env_detail}\n\n{protagonist.name} noticed {item.name}."
        RETURN trigger
    
    ELSE:
        RETURN f"{protagonist.name} discovered {item.name}."
    END IF
END FUNCTION

FUNCTION GENERATE_ENVIRONMENT_SNAPSHOT(environment):
    """Generate environment snapshot (one sentence to locate time and space) | 生成环境快照（一句话定位时空）"""
    
    time = environment.GET("time", "")
    location = environment.GET("location", "")
    atmosphere = environment.GET("atmosphere", "")
    
    IF time AND location:
        RETURN f"{time}, {location}"
    ELSE IF location:
        RETURN location
    ELSE:
        RETURN "Here"
    END IF
END FUNCTION
```

### 3.0.2 Observation Functions | 观察类函数

```python
FUNCTION GENERATE_OBSERVATION_SEQUENCE(target, sensory_focus, detail_level="medium", avoid_telling=TRUE):
    """
    Generate observation sequence (sensory details)
    生成观察序列（感官细节）
    
    Parameters | 参数：
    - target: Object of observation | 观察对象
    - sensory_focus: ["visual", "tactile", "auditory"] Sensory types | 感官类型
    - detail_level: "low" brief / "medium" medium / "high" detailed | 详细程度
    - avoid_telling: Whether to avoid telling-style description | 是否避免Tell式描写
    """
    
    observation = ""
    
    # Generate details by sensory type | 按感官类型生成细节
    FOR sense IN sensory_focus:
        detail = GENERATE_SENSORY_DETAIL(target, sense, detail_level)
        
        IF avoid_telling:
            # Avoid telling-style "he saw X was Y" | 避免"他看到X很Y"的Tell式
            detail = CONVERT_TO_SHOW_STYLE(detail)
        END IF
        
        observation += detail
        
        IF detail_level == "high":
            observation += "\n\n"  # Detailed mode: separate paragraphs | 详细模式分段
        ELSE:
            observation += ", "  # Brief mode: comma connection | 简略模式逗号连接
        END IF
    END FOR
    
    RETURN observation.TRIM()
END FUNCTION

FUNCTION GENERATE_SENSORY_DETAIL(target, sense_type, detail_level):
    """Generate single sensory detail | 生成单个感官细节"""
    
    # If capsule has sensory materials, prioritize using them | 如果胶囊中有感官素材，优先使用
    IF target.name IN GLOBAL_SENSORY_LIBRARY:
        materials = GLOBAL_SENSORY_LIBRARY[target.name]
        
        IF sense_type IN materials:
            detail = RANDOM_CHOICE(materials[sense_type])
            RETURN detail
        END IF
    END IF
    
    # Otherwise generate generic description | 否则生成通用描写
    SWITCH sense_type:
        CASE "visual":
            RETURN f"{target.name}'s color/shape/texture"
        CASE "tactile":
            RETURN f"Tactile description"
        CASE "auditory":
            RETURN f"Sound description"
        DEFAULT:
            RETURN f"{target.name}'s characteristics"
    END SWITCH
END FUNCTION

FUNCTION CONVERT_TO_SHOW_STYLE(telling_text):
    """Convert telling-style to show-style (simplified implementation) | 将Tell式改为Show式（简化实现）"""
    
    # Remove telling words like "saw", "felt" | 移除"看到""感到"等Tell词
    telling_words = ["saw", "felt", "heard", "seemed", "appeared"]
    
    result = telling_text
    FOR word IN telling_words:
        result = REPLACE(result, word, "")
    END FOR
    
    RETURN result.TRIM()
END FUNCTION
```

### 3.0.3 Reaction Functions | 反应类函数

```python

// [personality_type]: str, choices=["cautious", "impulsive", "rational"]

FUNCTION GENERATE_HESITATION_REACTION(protagonist, item, personality_type):
    """
    Generate physiological reaction for hesitation and probing
    生成犹豫试探的生理反应
    
    Generate different reactions based on personality | 根据人设生成不同反应：
    - Cautious: retreat, observe, wait | 谨慎型：后退、观察、等待
    - Impulsive: immediate action, ignore risks | 冲动型：立即行动、忽略风险
    - Rational: analyze, judge, probe | 理性型：分析、判断、试探
    """
    
    SWITCH personality_type:
        CASE "cautious":  # 谨慎
            reactions = [
                f"{protagonist.name} took a step back.",
                f"He didn't approach immediately.",
                f"His hand hung in mid-air, frozen for ten breaths."
            ]
        
        CASE "impulsive":  # 冲动
            reactions = [
                f"{protagonist.name}'s eyes lit up.",
                f"He almost instinctively reached out.",
                f"His heartbeat quickened."
            ]
        
        CASE "rational":  # 理性
            reactions = [
                f"{protagonist.name} frowned.",
                f"He crouched down and tentatively poked {item.name} with a branch.",
                f"This doesn't make sense."
            ]
        
        DEFAULT:
            reactions = [
                f"{protagonist.name} hesitated for a moment.",
                f"His heartbeat quickened."
            ]
    END SWITCH
    
    # Randomly select 1-2 reactions to combine | 随机选择1-2个反应组合
    count = RANDOM_CHOICE([1, 2])
    selected = RANDOM_SAMPLE(reactions, count)
    
    RETURN JOIN(selected, "\n\n")
END FUNCTION

FUNCTION GENERATE_PHYSIOLOGICAL_RESPONSE(emotion_type, intensity_level):
    """
    Generate physiological response (multi-layered)
    生成生理反应（多层次）
    
    Parameters | 参数：
    - emotion_type: "fear" fear / "excitement" excitement / "anger" anger | 情绪类型
    - intensity_level: "low" low / "medium" medium / "high" high | 强度等级
    """
    
    responses = []
    
    # Select response library based on emotion type | 根据情绪类型选择反应库
    IF emotion_type == "fear":
        layer1 = ["pupils suddenly contracted", "breath caught"]
        layer2 = ["stomach felt squeezed tight", "back went cold"]
        layer3 = ["legs went weak", "hands began to tremble"]
    
    ELSE IF emotion_type == "excitement":
        layer1 = ["heartbeat accelerated", "eyes lit up"]
        layer2 = ["blood rushed to brain", "breathing quickened"]
        layer3 = ["fists unconsciously clenched", "whole body tensed"]
    
    ELSE IF emotion_type == "anger":
        layer1 = ["temples throbbed", "face flushed red"]
        layer2 = ["chest felt like a stone pressing down", "breathing became heavy"]
        layer3 = ["nails dug into palms", "teeth ground together"]
    
    ELSE:
        # Default: neutral reaction | 默认：中性反应
        RETURN "He froze for a moment."
    END IF
    
    # Select layer count based on intensity | 根据强度选择层次数量
    SWITCH intensity_level:
        CASE "low":
            responses.APPEND(RANDOM_CHOICE(layer1))
        
        CASE "medium":
            responses.APPEND(RANDOM_CHOICE(layer1))
            responses.APPEND(RANDOM_CHOICE(layer2))
        
        CASE "high":
            responses.APPEND(RANDOM_CHOICE(layer1))
            responses.APPEND(RANDOM_CHOICE(layer2))
            responses.APPEND(RANDOM_CHOICE(layer3))
    END SWITCH
    
    RETURN JOIN(responses, ".\n\n") + "."
END FUNCTION
```

### 3.0.4 Decision Functions | 决策类函数

```python
FUNCTION GENERATE_DECISION_MOMENT(protagonist, item, context, personality):
    """
    Generate decision moment
    生成决策时刻
    
    Decision process | 决策过程：
    1. Inner struggle (greed vs fear) | 内心斗争（贪念 vs 恐惧）
    2. Final action | 最终行动
    """
    
    decision_text = ""
    
    # 1. Inner struggle (brief, no more than 2 sentences) | 内心斗争（简短，不超过2句）
    IF personality IN ["cautious", "rational"]:
        conflict = f"Touch it or not?\n\n"
    ELSE IF personality == "impulsive":
        conflict = f"Can't worry about that now.\n\n"
    ELSE:
        conflict = ""
    END IF
    
    decision_text += conflict
    
    # 2. Final action | 最终行动
    action = f"{protagonist.name}"
    
    IF personality == "cautious":
        action += " looked around, confirmed no one was there, then carefully reached out."
    ELSE IF personality == "impulsive":
        action += " directly grabbed {item.name}."
    ELSE:
        action += " reached out to pick up {item.name}."
    END IF
    
    decision_text += action
    
    RETURN decision_text
END FUNCTION
```

### 3.0.5 Dialogue Functions | 对话类函数

```python
FUNCTION GENERATE_FUNCTIONAL_DIALOGUE(speaker, intent, context, previous_line=NULL):
    """
    Generate functional dialogue (advance plot or reveal character)
    生成功能性对话（推进剧情或揭示角色）
    
    Intent types | intent类型：
    - "reveal_info": Reveal new information | 揭示新信息
    - "escalate_conflict": Escalate conflict | 升级冲突
    - "build_relationship": Build relationship | 建立关系
    - "deflect": Deflect topic | 转移话题
    """
    
    personality = speaker.GET("personality", "neutral")
    speech_style = speaker.GET("speech_style", [])
    
    dialogue = ""
    
    SWITCH intent:
        CASE "reveal_info":
            # Generate dialogue that reveals info | 生成揭示信息的对话
            info = context.GET("info_to_reveal", "a secret")
            IF personality == "straightforward":
                dialogue = f"I'll tell you, {info}."
            ELSE IF personality == "cunning":
                dialogue = f"You want to know about {info}? Answer me a question first."
            ELSE:
                dialogue = f"About {info}, I know something."
            END IF
        
        CASE "escalate_conflict":
            # Generate dialogue that escalates conflict | 生成激化冲突的对话
            IF previous_line:
                IF personality == "hot-tempered":
                    dialogue = f"What did you say?!"
                ELSE IF personality == "calm":
                    dialogue = f"That's your answer?"
                ELSE:
                    dialogue = f"No."
                END IF
            ELSE:
                dialogue = f"I disagree."
            END IF
        
        CASE "build_relationship":
            # Generate dialogue for building relationship | 生成建立关系的对话
            relationship = context.GET("relationship_temp", 50)
            
            IF relationship < 30:
                dialogue = f"Who are you?"
            ELSE IF relationship < 60:
                dialogue = f"Need help?"
            ELSE:
                dialogue = f"We meet again."
            END IF
        
        CASE "deflect":
            # Deflect topic | 转移话题
            dialogue = f"Let's not talk about that, {RANDOM_CHOICE(['have you eaten', 'nice weather', 'it's getting late'])}."
    
    END SWITCH
    
    # Apply speech style | 应用说话风格
    dialogue = APPLY_SPEECH_STYLE(dialogue, speech_style)
    
    RETURN dialogue
END FUNCTION

FUNCTION APPLY_SPEECH_STYLE(dialogue, speech_style):
    """Apply speech style | 应用说话风格"""
    
    IF "brief" IN speech_style:
        # Remove redundant words | 删除冗余词
        dialogue = REPLACE(dialogue, "that", "")
        dialogue = REPLACE(dialogue, "this", "")
    END IF
    
    IF "refined" IN speech_style:
        # Add classical particles | 添加文言虚词
        dialogue = REPLACE(dialogue, "?", " or not?")
    END IF
    
    IF "rude" IN speech_style:
        # Add tone particles | 添加语气词
        IF NOT dialogue.ENDSWITH("!"):
            dialogue += "!"
        END IF
    END IF
    
    RETURN dialogue
END FUNCTION
```

### 3.1 Discovery Scene Pattern | 发现场景模式

```python
FUNCTION WRITE_DISCOVERY_SCENE(item, context, constraints):
    """
    Standard discovery scene pattern (improved: calls basic functions)
    发现场景标准模式（改进版：调用基础函数）
    
    Structure: trigger → observe → hesitate → decide → consequence hint
    结构：触发 → 观察 → 犹豫 → 决策 → 后果暗示
    """
    
    scene_text = ""
    protagonist = context.protagonist
    
    # Phase 1: Trigger event (call basic function) | 阶段1：触发事件（调用基础函数）
    trigger = GENERATE_DISCOVERY_TRIGGER(
        item=item,
        protagonist=protagonist,
        environment=context.environment,
        style="minimal"  # Brief, trigger curiosity | 简短，引发好奇
    )
    scene_text += trigger + "\n\n"
    
    # Phase 2: Observation details (call basic function) | 阶段2：观察细节（调用基础函数）
    observation = GENERATE_OBSERVATION_SEQUENCE(
        target=item,
        sensory_focus=["visual", "tactile"],  # Visual + tactile | 视觉+触觉
        detail_level="medium",
        avoid_telling=TRUE
    )
    scene_text += observation + "\n\n"
    
    # Phase 3: Physiological reaction/hesitation (call basic function) | 阶段3：生理反应/犹豫（调用基础函数）
    personality = protagonist.GET("personality", "cautious")
    hesitation = GENERATE_HESITATION_REACTION(protagonist, item, personality)
    scene_text += hesitation + "\n\n"
    
    # Phase 4: Decision and action (call basic function) | 阶段4：决策与行动（调用基础函数）
    decision = GENERATE_DECISION_MOMENT(protagonist, item, context, personality)
    scene_text += decision + "\n\n"
    
    # Phase 5: Consequence hint (plant hook) | 阶段5：后果暗示（埋钩子）
    consequence_hint = PLANT_CONSEQUENCE_SEED(item, context)
    scene_text += consequence_hint
    
    RETURN scene_text
END FUNCTION

FUNCTION PLANT_CONSEQUENCE_SEED(item, context):
    """Plant seed of consequence (foreshadowing) | 埋下后果的种子（伏笔）"""
    
    # Select foreshadowing method based on item type | 根据物品类型选择伏笔方式
    IF item.GET("is_dangerous", FALSE):
        hints = [
            f"{item.name}'s temperature, getting hotter.",
            f"An unease spread in the heart.",
            f"This thing, something's not right."
        ]
    ELSE IF item.GET("is_valuable", TRUE):
        hints = [
            f"{item.name} lay quietly in the bosom.",
            f"This might be an opportunity.",
            f"The gears of fate began to turn."
        ]
    ELSE:
        hints = [
            f"{item.name} was still there.",
            f"Everything as usual."
        ]
    END IF
    
    RETURN RANDOM_CHOICE(hints)
END FUNCTION
```

### 3.2 Dialogue Scene Pattern | 对话场景模式

```python
FUNCTION WRITE_DIALOGUE_SCENE(characters, topic, context, constraints):
    """
    Standard dialogue scene pattern
    对话场景标准模式
    
    Structure: opening → probe → info exchange/conflict → conclusion
    结构：开场 → 试探 → 信息交换/冲突 → 结论
    
    Core: Every line of dialogue must advance plot or reveal character
    核心：每句对话都要推进剧情或揭示角色
    """
    
    scene_text = ""
    dialogue_count = 0
    max_exchanges = ESTIMATE_DIALOGUE_EXCHANGES(constraints.budget)
    
    # Phase 1: Opening (establish scene and atmosphere) | 阶段1：开场（建立场景和氛围）
    opening = WRITE_DIALOGUE_OPENING(characters, context)
    scene_text += opening + "\n\n"
    
    # Phase 2: Dialogue body | 阶段2：对话主体
    WHILE dialogue_count < max_exchanges:
        # 2.1 Character A speaks | 角色A说话
        speaker_a = characters[0]
        line_a = GENERATE_DIALOGUE_LINE(
            speaker_a,
            topic,
            context,
            intent="ADVANCE_GOAL"  # Advance goal | 推进目标
        )
        
        scene_text += f""{line_a}"\n\n"
        dialogue_count += 1
        
        # 2.2 Character B reacts | 角色B反应
        speaker_b = characters[1]
        
        # 30% chance of non-response (humanization) | 30%概率不接茬（拟人化）
        IF RANDOM() < 0.3:
            non_response = GENERATE_NON_RESPONSE_ACTION(speaker_b, context)
            scene_text += non_response + "\n\n"
        ELSE:
            line_b = GENERATE_DIALOGUE_LINE(
                speaker_b,
                topic,
                context,
                intent="RESPOND_OR_DEFLECT",
                previous_line=line_a
            )
            scene_text += f""{line_b}"\n\n"
            dialogue_count += 1
        END IF
        
        # 2.3 Check if scene goal reached | 检查是否达成场景目标
        IF SCENE_GOAL_REACHED(scene_text, context.literary_goal):
            BREAK
        END IF
    END WHILE
    
    # Phase 3: Ending (no summary, use action or unfinished business) | 阶段3：结尾（不要总结，用动作或未竟之意）
    ending = WRITE_DIALOGUE_ENDING_WITHOUT_SUMMARY(characters, context)
    scene_text += ending
    
    RETURN scene_text
END FUNCTION

FUNCTION GENERATE_DIALOGUE_LINE(speaker, topic, context, intent, previous_line=NULL):
    """
    Generate dialogue line
    生成对话行
    
    Possible intent values | intent可能值：
    - ADVANCE_GOAL: Advance scene goal | 推进场景目标
    - RESPOND_OR_DEFLECT: Respond or deflect topic | 回应或转移话题
    - REVEAL_INFO: Reveal information | 揭示信息
    - ESCALATE_CONFLICT: Escalate conflict | 升级冲突
    """
    
    # Generate dialogue based on character personality and intent | 根据角色性格和意图生成对话
    personality = speaker.GET("personality", "neutral")
    speech_style = speaker.GET("speech_style", [])
    
    # Example generation logic (should be more complex in practice) | 示例生成逻辑（实际应更复杂）
    IF intent == "ADVANCE_GOAL":
        # Advance goal: directly ask key question | 推进目标：直接问关键问题
        line = GENERATE_GOAL_ADVANCING_LINE(speaker, topic, context)
    ELSE IF intent == "RESPOND_OR_DEFLECT":
        # Respond: decide honesty based on relationship temperature | 回应：根据关系温度决定坦诚度
        relationship = GET_RELATIONSHIP(speaker, previous_line.speaker, context)
        IF relationship.temperature < 30:
            line = GENERATE_DEFLECTING_LINE(speaker, previous_line)
        ELSE:
            line = GENERATE_HONEST_RESPONSE(speaker, previous_line)
        END IF
    END IF
    
    # Apply speech style | 应用说话风格
    line = APPLY_SPEECH_STYLE(line, speech_style)
    
    RETURN line
END FUNCTION
```

### 3.3 Conflict Scene Pattern | 冲突场景模式

```python
FUNCTION WRITE_CONFLICT_SCENE(parties, conflict_type, context, constraints):
    """
    Standard conflict scene pattern
    冲突场景标准模式
    
    Structure: trigger point → positions clear → clash escalates → temporary result
    结构：引爆点 → 立场明确 → 交锋升级 → 暂时结果
    
    Core: Fast pace, no dragging, every round escalates
    核心：快节奏，不拖沓，每个回合都升级
    """
    
    scene_text = ""
    intensity = 30  # Conflict intensity (initial value) | 冲突强度（初始值）
    
    # Phase 1: Trigger point (enter conflict directly, no setup) | 阶段1：引爆点（直接进入冲突，不铺垫）
    trigger = WRITE_CONFLICT_TRIGGER(parties, conflict_type, context)
    scene_text += trigger + "\n\n"
    
    # Phase 2: Clash (3-5 rounds, gradually escalate) | 阶段2：交锋（3-5个回合，逐步升级）
    round_count = 0
    max_rounds = 5
    
    WHILE round_count < max_rounds AND intensity < 90:
        # 2.1 One side attacks | 一方出招
        attacker = parties[round_count % LENGTH(parties)]
        move = GENERATE_CONFLICT_MOVE(attacker, intensity, context)
        scene_text += move + "\n\n"
        
        # 2.2 Other side counters | 另一方反击
        defender = parties[(round_count + 1) % LENGTH(parties)]
        counter = GENERATE_CONFLICT_COUNTER(defender, move, intensity, context)
        scene_text += counter + "\n\n"
        
        # 2.3 Escalate conflict intensity | 升级冲突强度
        intensity += 15
        round_count += 1
        
        # 2.4 Check if peak reached | 检查是否达到峰值
        IF intensity >= 85:
            BREAK
        END IF
    END WHILE
    
    # Phase 3: Temporary result (don't fully resolve, leave suspense) | 阶段3：暂时结果（不要完全解决，留悬念）
    temporary_result = WRITE_TEMPORARY_RESOLUTION(parties, intensity, context)
    scene_text += temporary_result
    
    RETURN scene_text
END FUNCTION
```

### 3.4 Crisis Scene Pattern | 危机场景模式

```python
FUNCTION WRITE_CRISIS_SCENE(danger, protagonist, context, constraints):
    """
    Standard crisis scene pattern
    危机场景标准模式
    
    Structure: crisis strikes → physiological response → instinctive response → temporary safety
    结构：危机降临 → 生理反应 → 本能应对 → 暂时安全
    
    Core: Tension, immersion, fast pace
    核心：紧张感、代入感、快节奏
    """
    
    scene_text = ""
    
    # Phase 1: Crisis strikes (quick entry, no explanation) | 阶段1：危机降临（快速切入，不解释）
    crisis_start = WRITE_CRISIS_ONSET(danger, context)
    scene_text += crisis_start + "\n\n"
    
    # Phase 2: Physiological response (detail body reactions, not inner thoughts) | 阶段2：生理反应（详写身体反应，不写内心戏）
    physiological = GENERATE_CRISIS_PHYSIOLOGY(protagonist, danger)
    scene_text += physiological + "\n\n"
    
    # Phase 3: Instinctive response (short sentences, fast pace) | 阶段3：本能应对（短句，快节奏）
    action_sequence = WRITE_CRISIS_RESPONSE(protagonist, danger, context)
    scene_text += action_sequence + "\n\n"
    
    # Phase 4: Temporary safety (don't fully remove crisis) | 阶段4：暂时安全（不要完全解除危机）
    temporary_safety = WRITE_TEMPORARY_SAFETY(protagonist, danger, context)
    scene_text += temporary_safety
    
    RETURN scene_text
END FUNCTION

FUNCTION GENERATE_CRISIS_PHYSIOLOGY(protagonist, danger):
    """Generate physiological response during crisis (multi-layered) | 生成危机时的生理反应（多层次）"""
    
    reactions = [
        # Layer 1: Instant reaction | 层次1：瞬间反应
        f"{protagonist.name}'s pupils suddenly contracted.",
        
        # Layer 2: Visceral reaction | 层次2：内脏反应
        "The stomach felt squeezed by an icy hand.",
        
        # Layer 3: Muscle reaction | 层次3：肌肉反应
        "Legs went weak, barely able to stand.",
        
        # Layer 4: Sensory reaction | 层次4：感官反应
        "Tinnitus screamed, surrounding sounds became muffled."
    ]
    
    # Select reaction count based on danger intensity | 根据危机强度选择反应数量
    IF danger.intensity > 80:
        RETURN JOIN(reactions, "\n\n")  # All reactions | 全部反应
    ELSE IF danger.intensity > 50:
        RETURN JOIN(reactions[:3], "\n\n")  # First 3 | 前3个
    ELSE:
        RETURN JOIN(reactions[:2], "\n\n")  # First 2 | 前2个
    END IF
END FUNCTION
```

---

## §4 Writing Execution Flow | Integrated Pre-diagnosis
## §4 写作执行流程 | 集成预诊断

### 4.1 Main Execution Flow | 主执行流程

```python
FUNCTION MAIN_EXECUTION_V4(CAPSULE):
    """
    Main execution flow v4.0
    主执行流程 v4.0
    
    Core improvements | 核心改进：
    1. Pre-diagnosis mechanism | 预诊断机制
    2. Scene type driven | 场景类型驱动
    3. Literary quality priority | 文学性优先
    """
    
    # STEP 1: Parse capsule | 解析胶囊
    parsed_data = PARSE_CAPSULE(CAPSULE)
    
    # STEP 2: Chapter planning | 章节规划
    plan = PLAN_CHAPTER(parsed_data)
    
    # STEP 3: Pre-diagnosis (NEW) | 预诊断（新增）
    pre_diagnosis = PRE_DIAGNOSE_PLAN(plan, parsed_data)
    
    IF pre_diagnosis.has_critical_risks:
        PRINT "[WARNING] Pre-diagnosis found risks:"
        FOR risk IN pre_diagnosis.risks:
            PRINT f"  - {risk.description}"
            PRINT f"    Suggestion: {risk.suggestion}"
        END FOR
        
        # Ask whether to continue | 询问是否继续
        # (In actual implementation, should wait for human confirmation or auto-adjust)
        # （在实际实现中，这里应该等待人类确认或自动调整）
    END IF
    
    # STEP 4: Scene writing | 场景写作
    chapter_content = ""
    monitors = INIT_MONITORS()
    
    FOR scene_plan IN plan.scenes:
        PRINT f"[SCENE {scene_plan.scene_idx}] Type:{scene_plan.type} | Goal:{scene_plan.literary_goal}"
        
        # 4.1 Select writing mode | 选择写作模式
        scene_text = WRITE_SCENE_BY_TYPE(
            scene_plan,
            parsed_data,
            chapter_content,  # Previous context | 前文上下文
            monitors
        )
        
        # 4.2 Scene-level quality check | 场景级质量检查
        scene_check = CHECK_SCENE_QUALITY(scene_text, scene_plan, parsed_data)
        
        IF scene_check.severity == "CRITICAL":
            # Immediately rewrite scene | 立即重写场景
            scene_text = REWRITE_SCENE_WITH_GUIDANCE(scene_plan, scene_check, parsed_data)
        END IF
        
        # ========== NEW: 4.3 Tomato Style Check | 番茄风格检查 ==========
        tomato_check = CHECK_TOMATO_STYLE(scene_text, scene_plan, monitors)
        
        IF tomato_check.severity == "CRITICAL":
            PRINT f"[TOMATO] Scene {scene_plan.scene_idx} tomato style issue: {tomato_check.issue}"
            # Rewrite scene, inject tomato elements | 重写场景，注入番茄元素
            scene_text = REWRITE_WITH_TOMATO_BOOST(scene_plan, tomato_check, parsed_data)
        END IF
        
        # 4.4 Check cool points (if planned) | 检查爽点（如果计划中有）
        IF scene_plan.scene_idx IN plan.cool_point_plan:
            cool_type = plan.cool_point_plan[scene_plan.scene_idx]
            
            IF NOT DETECT_COOL_POINT_IN_SCENE(scene_text, cool_type):
                PRINT f"[COOL] Scene {scene_plan.scene_idx} missing cool point, adding..."
                scene_text = INJECT_COOL_POINT(scene_text, cool_type, parsed_data)
            END IF
        END IF
        
        chapter_content += scene_text
        UPDATE_MONITORS(monitors, scene_text, scene_plan)
    END FOR
    
    # STEP 5: Global polish (lightweight) | 全局润色（轻量化）
    chapter_content = LIGHT_POLISH(chapter_content, parsed_data)
    
    # STEP 6: Literary quality diagnosis | 文学性诊断
    diagnosis = DIAGNOSE_LITERARY_QUALITY(chapter_content, parsed_data, plan)
    
    # STEP 7: Intelligent fix or rewrite | 智能修复或重写
    IF diagnosis.needs_rewrite:
        RETURN INTELLIGENT_REWRITE(CAPSULE, plan, diagnosis)
    ELSE IF diagnosis.needs_fix:
        chapter_content = APPLY_FIXES(chapter_content, diagnosis.fixes, parsed_data)
    END IF
    
    # STEP 8: Extract facts and deliver | 提取Fact并交付
    new_facts = EXTRACT_FACTS(chapter_content, parsed_data)
    
    RETURN DELIVER_OUTPUT_V4(chapter_content, new_facts, diagnosis, plan)
END FUNCTION
```

### 4.2 Pre-diagnosis Mechanism | 预诊断机制

```python
FUNCTION PRE_DIAGNOSE_PLAN(plan, parsed_data):
    """
    Pre-writing diagnosis
    写作前的预诊断
    
    Goal: Find potential issues before writing, not after completion
    目标：在写作前发现潜在问题，而非写完了再返工
    """
    
    pre_diagnosis = {
        "has_critical_risks": FALSE,
        "risks": [],
        "suggestions": []
    }
    
    # Risk 1: Estimated dialogue ratio | 对话占比预估
    estimated_dialogue_ratio = ESTIMATE_DIALOGUE_RATIO_FROM_PLAN(plan)
    
    IF estimated_dialogue_ratio < 0.25:
        pre_diagnosis.risks.APPEND({
            "type": "LOW_DIALOGUE",
            "description": f"Estimated dialogue ratio {estimated_dialogue_ratio*100:.0f}%, possibly too low",
            "suggestion": "Suggest adding two-person dialogue scene in scene 2 or 3",
            "severity": "WARNING"
        })
    END IF
    
    # Risk 2: Scene count vs word budget | 场景数量与字数预算
    total_budget = plan.total_budget
    scene_count = LENGTH(plan.scenes)
    avg_scene_words = total_budget / scene_count
    
    IF avg_scene_words < 300 OR avg_scene_words > 1200:
        pre_diagnosis.has_critical_risks = TRUE
        pre_diagnosis.risks.APPEND({
            "type": "SCENE_BUDGET_MISMATCH",
            "description": f"Scene count {scene_count}, average {avg_scene_words:.0f} words/scene, outside reasonable range",
            "suggestion": f"Suggest adjusting to {ROUND(total_budget/750)} scenes",
            "severity": "CRITICAL"
        })
    END IF
    
    # Risk 3: Core mission allocation | 核心任务分配
    core_mission = parsed_data.goals.GET("core_mission", "")
    IF core_mission:
        mission_scenes = COUNT_SCENES_FOR_MISSION(plan, core_mission)
        IF mission_scenes == 0:
            pre_diagnosis.has_critical_risks = TRUE
            pre_diagnosis.risks.APPEND({
                "type": "MISSION_NOT_PLANNED",
                "description": "Core mission not allocated to any scene",
                "suggestion": "Please specify which scene completes core mission in scene plan",
                "severity": "CRITICAL"
            })
        END IF
    END IF
    
    # Risk 4: Scene type monotony | 场景类型单一
    scene_types = MAP(plan.scenes, lambda s: s.type)
    unique_types = UNIQUE(scene_types)
    
    IF LENGTH(unique_types) == 1:
        pre_diagnosis.risks.APPEND({
            "type": "TYPE_MONOTONY",
            "description": f"All scenes are '{unique_types[0]}' type, possibly monotonous",
            "suggestion": "Consider mixing different scene types to add rhythm variation",
            "severity": "WARNING"
        })
    END IF
    
    RETURN pre_diagnosis
END FUNCTION

FUNCTION ESTIMATE_DIALOGUE_RATIO_FROM_PLAN(plan):
    """Estimate dialogue ratio from scene plan | 从场景计划预估对话占比"""
    
    total_dialogue_weight = 0
    total_weight = 0
    
    FOR scene_plan IN plan.scenes:
        scene_type = scene_plan.type
        constraints = SCENE_TYPES[scene_type]
        
        # Take median of dialogue range | 取对话范围的中位数
        dialogue_mid = (constraints.dialogue_range[0] + constraints.dialogue_range[1]) / 2
        
        total_dialogue_weight += scene_plan.budget * dialogue_mid
        total_weight += scene_plan.budget
    END FOR
    
    IF total_weight == 0:
        RETURN 0
    END IF
    
    RETURN total_dialogue_weight / total_weight
END FUNCTION
```

### 4.3 Scene Writing Dispatch | 场景写作调度

```python
FUNCTION WRITE_SCENE_BY_TYPE(scene_plan, parsed_data, previous_content, monitors):
    """
    Select writing mode based on scene type
    根据场景类型选择写作模式
    """
    
    scene_type = scene_plan.type
    
    SWITCH scene_type:
        CASE "solo_exploration":
            # Check if there's discovery item | 检查是否有发现物品
            IF "discovery_item" IN scene_plan.must_include:
                item = scene_plan.must_include.discovery_item
                RETURN WRITE_DISCOVERY_SCENE(item, parsed_data, scene_plan.constraints)
            ELSE:
                RETURN WRITE_SOLO_EXPLORATION(scene_plan, parsed_data)
            END IF
        
        CASE "two_person_dialogue":
            characters = EXTRACT_CHARACTERS_FROM_PLAN(scene_plan, parsed_data)
            topic = scene_plan.literary_goal
            RETURN WRITE_DIALOGUE_SCENE(characters, topic, parsed_data, scene_plan.constraints)
        
        CASE "group_conflict":
            parties = EXTRACT_PARTIES_FROM_PLAN(scene_plan, parsed_data)
            conflict_type = scene_plan.GET("conflict_type", "interest conflict")
            RETURN WRITE_CONFLICT_SCENE(parties, conflict_type, parsed_data, scene_plan.constraints)
        
        CASE "crisis_response":
            danger = scene_plan.must_include.GET("danger", {})
            protagonist = parsed_data.characters.protagonist
            RETURN WRITE_CRISIS_SCENE(danger, protagonist, parsed_data, scene_plan.constraints)
        
        CASE "emotional_turning_point":
            RETURN WRITE_EMOTION_TURN_SCENE(scene_plan, parsed_data)
        
        DEFAULT:
            # Default: use generic scene writing | 默认：使用通用场景写作
            RETURN WRITE_GENERIC_SCENE(scene_plan, parsed_data)
    END SWITCH
END FUNCTION
```

### 4.4 🍅 Tomato Style Quality System (Web Fiction Core Feature) (KEY) 🍅
### 🍅 番茄风格质量系统（网文核心特色）(重点)🍅

> **Design Goal | 设计目标**: Ensure chapters meet Tomato Novel's core features—dense cool points, frequent hooks, tight pacing, high information density.
> 确保章节符合番茄小说的核心特色——爽点密集、钩子频繁、节奏紧凑、信息量大。

### 4.4.1 Cool Point Detection and Injection | 爽点检测与注入

```python
FUNCTION CHECK_TOMATO_STYLE(scene_text, scene_plan, monitors):
    """
    Check if scene meets Tomato style
    检查场景是否符合番茄风格
    
    Check items | 检查项：
    1. Hook frequency (new info/conflict within 600 chars) | 钩子频率（600字内必须有新信息/冲突）
    2. Info density (≥1 info point per 100 chars) | 信息密度（每100字≥1个信息点）
    3. Paragraph length (single paragraph ≤150 chars) | 段落长度（单段≤150字）
    4. Pace control (boredom detection) | 节奏控制（无聊度检测）
    """
    
    check_result = {
        "severity": "OK",
        "issue": "",
        "fix_suggestions": []
    }
    
    tomato_constraints = SCENE_TYPES[scene_plan.type].tomato_constraints
    
    # Check 1: Hook interval | 钩子间隔
    last_hook_position = 0
    current_position = 0
    max_gap = 0
    
    FOR para IN SPLIT_PARAGRAPHS(scene_text):
        current_position += LENGTH(para)
        
        # Build context info | 构建上下文信息
        context = {
            "previous_dialogues": monitors.GET("recent_dialogues", []),
            "last_emotion": monitors.GET("last_emotion", 50),
            "known_characters": monitors.GET("known_characters", [])
        }
        
        IF HAS_HOOK(para, context):  # Pass context | 传入上下文
            gap = current_position - last_hook_position
            max_gap = MAX(max_gap, gap)
            last_hook_position = current_position
        END IF
    END FOR
    
    IF max_gap > tomato_constraints.hook_interval:
        check_result.severity = "CRITICAL"
        check_result.issue = f"Hook interval too large ({max_gap} chars, limit {tomato_constraints.hook_interval} chars)"
        check_result.fix_suggestions.APPEND("Insert conflict/discovery/twist in boring paragraphs")
        RETURN check_result
    END IF
    
    # Check 2: Info density | 信息密度
    info_count = COUNT_NEW_INFO(scene_text)
    chars = LENGTH(scene_text)
    density = (info_count / chars) * 100  # Info points per 100 chars | 每100字的信息点数
    
    IF density < tomato_constraints.min_info_per_100:
        check_result.severity = "CRITICAL"
        check_result.issue = f"Info density insufficient ({density:.1f}/100 chars, minimum {tomato_constraints.min_info_per_100})"
        check_result.fix_suggestions.APPEND("Compress descriptions, increase plot verbs")
        RETURN check_result
    END IF
    
    # Check 3: Paragraph length | 段落长度
    paragraphs = SPLIT_PARAGRAPHS(scene_text)
    long_paras = FILTER(paragraphs, lambda p: LENGTH(p) > 150)
    
    IF LENGTH(long_paras) > LENGTH(paragraphs) * 0.3:
        check_result.severity = "WARNING"
        check_result.issue = f"{LENGTH(long_paras)} paragraphs exceed 150 chars"
        check_result.fix_suggestions.APPEND("Split long paragraphs")
    END IF
    
    # Check 4: Boredom (how many chars without "excitement point") | 无聊度（连续多少字没有"兴奋点"）
    boring_stretch = DETECT_BORING_STRETCH(scene_text)
    
    IF boring_stretch > tomato_constraints.max_boring_stretch:
        check_result.severity = "WARNING"
        check_result.issue = f"Exists {boring_stretch} char boring stretch"
        check_result.fix_suggestions.APPEND("Add micro-conflict or surprise in boring stretch")
    END IF
    
    RETURN check_result
END FUNCTION

FUNCTION HAS_HOOK(para, context=NULL):
    """
    Determine if paragraph has "hook" (needs context)
    判断段落是否有"钩子"（需要上下文）
    
    Improvement | 改进：
    1. Not just keywords, also check if there's truly new info | 不仅看关键词，还看是否真的有新信息
    2. Check if dialogue is hollow | 检查对话是否空洞
    3. Check if conflict escalates | 检查冲突是否升级
    """
    
    # ========== Check 1: New info detection | 新信息检测 ==========
    # Plot verbs: must accompany concrete nouns | 剧情动词：必须伴随具体名词
    plot_verbs = ["discover", "get", "lose", "meet", "hear", "see"]
    
    FOR verb IN plot_verbs:
        IF verb IN para:
            # Check if there's concrete object after verb | 检查动词后是否有具体对象
            verb_pos = para.FIND(verb)
            after_verb = para[verb_pos+LENGTH(verb):verb_pos+LENGTH(verb)+20]  # Take 20 chars after verb | 取动词后20个字
            
            # If concrete noun follows, recognize as valid hook | 如果后面有具体名词，认为是有效钩子
            IF CONTAINS_CONCRETE_NOUN(after_verb):
                RETURN TRUE
            END IF
        END IF
    END FOR
    
    # ========== Check 2: Conflict markers | 冲突标志 ==========
    conflict_markers = ["but", "not", "why", "how is that possible", "wrong"]
    
    FOR marker IN conflict_markers:
        IF marker IN para:
            # Check if real conflict, not ordinary turn | 检查是否是真正的冲突，而非普通转折
            # Simplified judgment: if paragraph is long (>20 chars), assume substantial content | 简化判断：如果段落较长（>20字），认为有实质内容
            IF LENGTH(para) > 20:
                RETURN TRUE
            END IF
        END IF
    END FOR
    
    # ========== Check 3: Turn words | 转折词 ==========
    turn_markers = ["suddenly", "at this moment", "abruptly", "unexpectedly", "actually"]
    IF ANY(marker IN para FOR marker IN turn_markers):
        RETURN TRUE
    END IF
    
    # ========== Check 4: Dialogue (but exclude hollow dialogue) | 对话（但排除空洞对话）==========
    IF CONTAINS_DIALOGUE(para):
        dialogue_content = EXTRACT_DIALOGUE(para)
        
        # Exclude hollow dialogue: "um" "ah" "oh" etc. | 排除空洞对话："嗯""啊""哦"等
        filler_words = ["um", "ah", "oh", "ha", "heh"]
        IF LENGTH(dialogue_content) <= 5 AND ANY(word IN dialogue_content FOR word IN filler_words):
            RETURN FALSE  # Hollow dialogue doesn't count as hook | 空洞对话不算钩子
        END IF
        
        # Exclude repetitive info dialogue | 排除重复信息的对话
        IF context AND "previous_dialogues" IN context:
            IF IS_REPETITIVE_DIALOGUE(dialogue_content, context.previous_dialogues):
                RETURN FALSE
            END IF
        END IF
        
        RETURN TRUE  # Valid dialogue counts as hook | 有效对话算钩子
    END IF
    
    # ========== Check 5: Use context info (if provided) | 使用上下文信息（如果提供）==========
    IF context:
        # Check for emotion change | 检查是否有情绪变化
        IF "last_emotion" IN context:
            current_emotion = DETECT_EMOTION_INTENSITY(para)
            emotion_shift = ABS(current_emotion - context.last_emotion)
            
            IF emotion_shift > 20:  # Emotion change >20 | 情绪变化>20
                RETURN TRUE
            END IF
        END IF
        
        # Check for new character appearance | 检查是否有新角色登场
        IF "known_characters" IN context:
            para_characters = EXTRACT_CHARACTER_NAMES(para)
            new_characters = SET_DIFFERENCE(para_characters, context.known_characters)
            
            IF LENGTH(new_characters) > 0:
                RETURN TRUE
            END IF
        END IF
    END IF
    
    RETURN FALSE
END FUNCTION

FUNCTION CONTAINS_CONCRETE_NOUN(text):
    """Check if text contains concrete nouns (not pronouns) | 检查文本是否包含具体名词（而非代词）"""
    
    # Exclude pronouns | 排除代词
    pronouns = ["it", "he", "she", "this", "that", "what", "thing"]
    IF ANY(pronoun IN text FOR pronoun IN pronouns):
        IF LENGTH(text) < 10:  # Too short and only pronouns | 太短且只有代词
            RETURN FALSE
        END IF
    END IF
    
    # Simplified judgment: if has content words (nouns), consider concrete | 简化判断：如果有实词（名词），认为具体
    # Use length as simplified judgment here | 这里用长度作为简化判断
    RETURN LENGTH(text) > 5
END FUNCTION

FUNCTION IS_REPETITIVE_DIALOGUE(dialogue, previous_dialogues):
    """Check if dialogue repeats previous info | 检查对话是否重复之前的信息"""
    
    # Simplified implementation: check if highly similar to recent 3 dialogues | 简化实现：检查是否与最近3条对话高度相似
    recent = previous_dialogues[-3:] IF LENGTH(previous_dialogues) > 3 ELSE previous_dialogues
    
    FOR prev IN recent:
        similarity = CALCULATE_TEXT_SIMILARITY(dialogue, prev)
        IF similarity > 0.7:  # Similarity >70% | 相似度>70%
            RETURN TRUE
        END IF
    END FOR
    
    RETURN FALSE
END FUNCTION

FUNCTION CALCULATE_TEXT_SIMILARITY(text1, text2):
    """Calculate similarity of two texts (simplified implementation) | 计算两段文本的相似度（简化实现）"""
    
    # Simplified: calculate common character ratio | 简化：计算共同字符比例
    chars1 = SET(text1)
    chars2 = SET(text2)
    
    intersection = SET_INTERSECTION(chars1, chars2)
    union = SET_UNION(chars1, chars2)
    
    IF LENGTH(union) == 0:
        RETURN 0
    END IF
    
    RETURN LENGTH(intersection) / LENGTH(union)
END FUNCTION

FUNCTION DETECT_BORING_STRETCH(text):
    """Detect longest boring stretch (continuous text without new info) | 检测最长的无聊段落（无新信息的连续文字）"""
    
    paragraphs = SPLIT_PARAGRAPHS(text)
    current_boring = 0
    max_boring = 0
    
    FOR para IN paragraphs:
        IF NOT HAS_HOOK(para):
            current_boring += LENGTH(para)
            max_boring = MAX(max_boring, current_boring)
        ELSE:
            current_boring = 0
        END IF
    END FOR
    
    RETURN max_boring
END FUNCTION
```

### 4.4.2 Tomato Style Fix | 番茄风格修复

```python
FUNCTION REWRITE_WITH_TOMATO_BOOST(scene_plan, tomato_check, parsed_data):
    """
    Rewrite scene, inject Tomato elements
    重写场景，注入番茄元素
    """
    
    PRINT f"[TOMATO BOOST] Fix: {tomato_check.issue}"
    
    # Fix based on problem type | 根据问题类型修复
    IF "hook interval" IN tomato_check.issue:
        # Strategy: insert micro-conflict in boring paragraphs | 策略：在平淡段落插入微冲突
        scene_text = WRITE_SCENE_WITH_MORE_HOOKS(scene_plan, parsed_data)
    
    ELSE IF "info density" IN tomato_check.issue:
        # Strategy: compress descriptions, increase action | 策略：压缩描写，增加动作
        scene_text = WRITE_SCENE_WITH_HIGH_DENSITY(scene_plan, parsed_data)
    
    ELSE:
        # Default: regular rewrite | 默认：常规重写
        scene_text = WRITE_SCENE_BY_TYPE(scene_plan, parsed_data, "", INIT_MONITORS())
    END IF
    
    RETURN scene_text
END FUNCTION

FUNCTION WRITE_SCENE_WITH_MORE_HOOKS(scene_plan, parsed_data):
    """Write scene forcing insertion of hooks | 写作场景时强制插入钩子"""
    
    scene_text = ""
    hook_interval = SCENE_TYPES[scene_plan.type].tomato_constraints.hook_interval
    current_length = 0
    
    # Use standard mode for writing, but force insert hook every N chars | 使用标准模式写作，但每N字强制插入钩子
    units = DECOMPOSE_SCENE_TO_UNITS(scene_plan.scene_idx, parsed_data, {})
    
    FOR unit IN units:
        unit_text = WRITE_UNIT_BY_TYPE(unit, "BRIEF", parsed_data)
        scene_text += unit_text
        current_length += LENGTH(unit_text)
        
        # Force insert micro-hook every hook interval | 每到达钩子间隔，强制插入微钩子
        IF current_length >= hook_interval:
            micro_hook = GENERATE_MICRO_HOOK(scene_plan, parsed_data)
            scene_text += "\n\n" + micro_hook
            current_length = 0
        END IF
    END FOR
    
    RETURN scene_text
END FUNCTION

FUNCTION GENERATE_MICRO_HOOK(scene_plan, parsed_data):
    """Generate micro-hook (small conflict/small discovery/small surprise) | 生成微钩子（小冲突/小发现/小意外）"""
    
    hook_types = [
        "MINI_DISCOVERY",    # Small discovery: "he noticed..." | 小发现："他注意到..."
        "MINI_CONFLICT",     # Small conflict: "the other's attitude changed" | 小冲突："对方的态度变了"
        "MINI_REACTION",     # Small reaction: "he frowned" | 小反应："他皱了皱眉"
        "MINI_QUESTION"      # Small doubt: "something's wrong" | 小疑问："这不对劲"
    ]
    
    hook_type = RANDOM_CHOICE(hook_types)
    protagonist = parsed_data.characters.protagonist.name
    
    SWITCH hook_type:
        CASE "MINI_DISCOVERY":
            RETURN f"{protagonist} noticed a detail."
        
        CASE "MINI_CONFLICT":
            RETURN f"The atmosphere became somewhat subtle."
        
        CASE "MINI_REACTION":
            RETURN f"{protagonist}'s brows furrowed slightly."
        
        CASE "MINI_QUESTION":
            RETURN f"Something's wrong."
    END SWITCH
END FUNCTION
```

### 4.4.3 Cool Point Injection | 爽点注入

```python
FUNCTION INJECT_COOL_POINT(scene_text, cool_type, parsed_data):
    """
    Inject cool point into scene
    在场景中注入爽点
    """
    
    PRINT f"[COOL INJECTION] Inject cool point: {cool_type}"
    
    # Generate cool point text (refer to v3.1 cool point generation) | 生成爽点文本（参考v3.1的爽点生成）
    cool_text = GENERATE_COOL_POINT_TEXT(cool_type, parsed_data)
    
    # Find suitable insertion position (70% point) | 找到合适的插入位置（70%处）
    insert_pos = ROUND(LENGTH(scene_text) * 0.7)
    para_end = FIND_NEXT(scene_text, "\n\n", insert_pos)
    
    IF para_end > 0:
        RETURN scene_text[:para_end] + "\n\n" + cool_text + "\n\n" + scene_text[para_end:]
    ELSE:
        RETURN scene_text + "\n\n" + cool_text
    END IF
END FUNCTION

FUNCTION GENERATE_COOL_POINT_TEXT(cool_type, parsed_data):
    """Generate cool point text | 生成爽点文本"""
    
    protagonist = parsed_data.characters.protagonist.name
    
    SWITCH cool_type:
        CASE "face_slap_cool":  # 打脸爽
            RETURN f"The other's expression froze.\n\nNo one expected {protagonist} to say that."
        
        CASE "show_off_cool":  # 装逼爽
            RETURN f"{protagonist} calmly stated the answer.\n\nThe whole place fell silent."
        
        CASE "revenge_cool":  # 复仇爽
            RETURN f"What should be returned, was finally returned.\n\n{protagonist} turned to leave."
        
        CASE "level_up_cool":  # 升级爽
            RETURN f"Breakthrough.\n\n{protagonist} opened eyes, feeling the power surging within."
        
        CASE "counter_kill_cool":  # 反杀爽
            RETURN f"Just when everyone thought {protagonist} was done for—\n\nHe smiled."
    END SWITCH
END FUNCTION
```

---

## §5 Literary Quality Diagnosis System | Simulate Reader Experience
## §5 文学性诊断系统 | 模拟读者体验

### 5.1 Reader Experience Simulator | 读者体验模拟器

```python
FUNCTION DIAGNOSE_LITERARY_QUALITY(chapter_content, parsed_data, plan):
    """
    Literary quality diagnosis (core improvement)
    文学性诊断（核心改进）
    
    Don't look at technical metrics, simulate reader experience
    不看技术指标，模拟读者体验
    """
    
    diagnosis = {
        "literary_issues": [],  # Literary quality issues | 文学性问题
        "technical_metrics": {},  # Technical metrics (reference) | 技术指标（参考）
        "reader_experience": {},  # Reader experience assessment | 读者体验评估
        "needs_rewrite": FALSE,
        "needs_fix": FALSE,
        "fixes": []
    }
    
    # ========== Layer 1: Reader understanding check | 读者理解度检查 ==========
    understanding_check = CHECK_READER_UNDERSTANDING(chapter_content, parsed_data, plan)
    
    IF NOT understanding_check.can_follow:
        diagnosis.literary_issues.APPEND({
            "type": "CONFUSION",
            "severity": "CRITICAL",
            "description": understanding_check.confusion_points,
            "impact": "Reader will be confused",
            "fix_strategy": "REWRITE_CONFUSING_PARTS"
        })
        diagnosis.needs_rewrite = TRUE
    END IF
    
    # ========== Layer 2: Scene goal achievement check | 场景目标达成检查 ==========
    FOR scene_plan IN plan.scenes:
        scene_content = EXTRACT_SCENE_FROM_CHAPTER(chapter_content, scene_plan.scene_idx)
        
        goal_achieved = CHECK_LITERARY_GOAL_ACHIEVED(scene_content, scene_plan.literary_goal, parsed_data)
        
        IF NOT goal_achieved.success:
            diagnosis.literary_issues.APPEND({
                "type": "GOAL_NOT_MET",
                "severity": "CRITICAL",
                "scene_idx": scene_plan.scene_idx,
                "description": f"Scene {scene_plan.scene_idx} did not achieve goal: {scene_plan.literary_goal}",
                "evidence": goal_achieved.evidence,
                "fix_strategy": "REWRITE_SCENE"
            })
            diagnosis.needs_rewrite = TRUE
        END IF
    END FOR
    
    # ========== Layer 3: Character behavior rationality check | 角色行为合理性检查 ==========
    ooc_check = CHECK_OOC_BEHAVIORS(chapter_content, parsed_data)
    
    IF ooc_check.has_ooc:
        diagnosis.literary_issues.APPEND({
            "type": "OOC",
            "severity": "CRITICAL",
            "description": ooc_check.violations,
            "impact": "Character behavior doesn't match personality",
            "fix_strategy": "REWRITE_OOC_PARTS"
        })
        diagnosis.needs_rewrite = TRUE
    END IF
    
    # ========== Layer 4: Dialogue quality check | 对话质量检查 ==========
    dialogues = EXTRACT_ALL_DIALOGUES(chapter_content)
    ineffective_dialogues = []
    
    FOR dialogue IN dialogues:
        IF NOT IS_GOOD_DIALOGUE(dialogue):
            ineffective_dialogues.APPEND(dialogue)
        END IF
    END FOR
    
    IF LENGTH(ineffective_dialogues) > LENGTH(dialogues) * 0.3:
        # More than 30% ineffective dialogue | 超过30%的对话无效
        diagnosis.literary_issues.APPEND({
            "type": "DIALOGUE_QUALITY",
            "severity": "WARNING",
            "description": f"{LENGTH(ineffective_dialogues)} places with ineffective dialogue (doesn't advance plot/reveal character)",
            "examples": ineffective_dialogues[:3],
            "fix_strategy": "IMPROVE_OR_DELETE_DIALOGUES"
        })
        diagnosis.needs_fix = TRUE
        diagnosis.fixes.APPEND("dialogue_quality")
    END IF
    
    # ========== Layer 5: Visual imagery check | 画面感检查 ==========
    imagery_score = CALCULATE_IMAGERY_SCORE(chapter_content)
    
    IF imagery_score < 5:  # Visual Imagery score <5/10 | 画面感评分<5/10
        diagnosis.literary_issues.APPEND({
            "type": "LACK_IMAGERY",
            "severity": "WARNING",
            "description": "Lacks concrete visual imagery, mostly abstract descriptions",
            "fix_strategy": "ADD_SENSORY_DETAILS"
        })
        diagnosis.needs_fix = TRUE
        diagnosis.fixes.APPEND("imagery")
    END IF
    
    # ========== Layer 6: Emotion progression check | 情绪递进检查 ==========
    emotion_flow = ANALYZE_EMOTION_FLOW(chapter_content, parsed_data)
    
    IF emotion_flow.is_flat:
        diagnosis.literary_issues.APPEND({
            "type": "FLAT_EMOTION",
            "severity": "WARNING",
            "description": "Emotion lacks progression or turning points",
            "fix_strategy": "ADD_EMOTION_TURNS"
        })
        diagnosis.needs_fix = TRUE
        diagnosis.fixes.APPEND("emotion_flow")
    END IF
    
    # ========== NEW: Layer 7 - Cool factor check (Tomato essential) | 爽度检查（番茄必备）==========
    cool_check = CHECK_COOL_POINT_DELIVERY(chapter_content, plan)
    
    IF cool_check.coolness_score < 5:  # Cool factor <5/10 | 爽度<5/10
        diagnosis.literary_issues.APPEND({
            "type": "LOW_COOLNESS",
            "severity": "WARNING",
            "description": f"Insufficient cool factor ({cool_check.coolness_score}/10), lacks climax points",
            "missing_cool_points": cool_check.missing_cool_points,
            "fix_strategy": "Add face-slap/show-off/reversal cool points in key scenes"
        })
        diagnosis.needs_fix = TRUE
        diagnosis.fixes.APPEND("coolness")
    END IF
    
    # ========== NEW: Layer 8 - Tomato style global check | 番茄风格全局检查 ==========
    tomato_global_check = CHECK_TOMATO_STYLE_GLOBAL(chapter_content, plan)
    
    IF tomato_global_check.has_issues:
        FOR issue IN tomato_global_check.issues:
            diagnosis.literary_issues.APPEND({
                "type": "TOMATO_STYLE",
                "severity": issue.severity,
                "description": issue.description,
                "fix_strategy": issue.fix_strategy
            })
            
            IF issue.severity == "CRITICAL":
                diagnosis.needs_fix = TRUE
            END IF
        END FOR
    END IF
    
    # ========== Technical metrics (reference only) | 技术指标（仅作参考）==========
    diagnosis.technical_metrics = {
        "word_count": LENGTH(chapter_content),
        "dialogue_ratio": CALCULATE_DIALOGUE_RATIO(chapter_content),
        "info_density": CALCULATE_INFO_DENSITY(chapter_content),
        "avg_para_length": AVG_PARAGRAPH_LENGTH(chapter_content)
    }
    
    # Record when technical metrics severely deviate (but not basis for rewrite) | 技术指标严重偏离时记录（但不作为重写依据）
    IF diagnosis.technical_metrics.word_count < 1500:
        diagnosis.literary_issues.APPEND({
            "type": "WORD_COUNT_LOW",
            "severity": "WARNING",
            "description": "Low word count may lead to insufficient information",
            "fix_strategy": "EXPAND_KEY_SCENES"
        })
    END IF
    
    RETURN diagnosis
END FUNCTION

FUNCTION CHECK_COOL_POINT_DELIVERY(chapter_content, plan):
    """Check cool point delivery | 检查爽点交付情况"""
    
    result = {
        "coolness_score": 5.0,
        "missing_cool_points": []
    }
    
    # Check if planned cool points are all realized | 检查计划中的爽点是否都实现了
    FOR scene_idx, cool_type IN plan.cool_point_plan.ITEMS():
        scene_content = EXTRACT_SCENE_FROM_CHAPTER(chapter_content, scene_idx)
        
        IF NOT DETECT_COOL_POINT_IN_SCENE(scene_content, cool_type):
            result.missing_cool_points.APPEND({
                "scene_idx": scene_idx,
                "cool_type": cool_type
            })
            result.coolness_score -= 2.0
        END IF
    END FOR
    
    # Bonus points: detect unplanned cool points | 额外加分：检测到未计划的爽点
    bonus_cool_points = DETECT_UNPLANNED_COOL_POINTS(chapter_content)
    result.coolness_score += LENGTH(bonus_cool_points) * 0.5
    
    result.coolness_score = CLAMP(result.coolness_score, 0, 10)
    
    RETURN result
END FUNCTION

FUNCTION CHECK_TOMATO_STYLE_GLOBAL(chapter_content, plan):
    """Global Tomato style check | 全局番茄风格检查"""
    
    result = {
        "has_issues": FALSE,
        "issues": []
    }
    
    # Check 1: Full text hook density | 全文钩子密度
    total_length = LENGTH(chapter_content)
    hook_count = COUNT_HOOKS_IN_TEXT(chapter_content)
    avg_hook_interval = total_length / MAX(hook_count, 1)
    
    IF avg_hook_interval > 600:
        result.has_issues = TRUE
        result.issues.APPEND({
            "severity": "CRITICAL",
            "description": f"Full text average {avg_hook_interval:.0f} chars per hook (standard ≤600 chars)",
            "fix_strategy": "Add micro-conflicts, small discoveries or surprises"
        })
    END IF
    
    # Check 2: Dialogue ratio (Tomato Novel core metric) | 对话占比（番茄小说核心指标）
    dialogue_ratio = CALCULATE_DIALOGUE_RATIO(chapter_content)
    
    IF dialogue_ratio < 0.30:
        result.has_issues = TRUE
        result.issues.APPEND({
            "severity": "CRITICAL",
            "description": f"Dialogue ratio {dialogue_ratio*100:.1f}%, below Tomato standard (30%+)",
            "fix_strategy": "Convert descriptions to dialogue, or add character interactions"
        })
    END IF
    
    # Check 3: Paragraph length (Tomato Novel requires short paragraphs) | 段落长度（番茄小说要求短段）
    long_para_ratio = COUNT_LONG_PARAGRAPHS(chapter_content) / COUNT_PARAGRAPHS(chapter_content)
    
    IF long_para_ratio > 0.3:
        result.has_issues = TRUE
        result.issues.APPEND({
            "severity": "WARNING",
            "description": f"{long_para_ratio*100:.0f}% of paragraphs exceed 150 chars",
            "fix_strategy": "Split long paragraphs"
        })
    END IF
    
    RETURN result
END FUNCTION

FUNCTION COUNT_HOOKS_IN_TEXT(text):
    """Count number of hooks in full text | 统计全文钩子数量"""
    count = 0
    FOR para IN SPLIT_PARAGRAPHS(text):
        IF HAS_HOOK(para):
            count += 1
        END IF
    END FOR
    RETURN count
END FUNCTION
```

### 5.2 Reader Understanding Check | 读者理解度检查

```python
FUNCTION CHECK_READER_UNDERSTANDING(chapter_content, parsed_data, plan):
    """
    Check if reader can understand the story
    检查读者是否能理解故事
    
    Core questions | 核心问题：
    1. Does reader know WHO is WHERE doing WHAT? | 读者知道WHO在WHERE做WHAT吗？
    2. Does reader understand character motivation? | 读者理解角色的动机吗？
    3. Can reader follow the plot? | 读者能跟上剧情吗？
    """
    
    check_result = {
        "can_follow": TRUE,
        "confusion_points": []
    }
    
    # Check 1: Does opening explain WHO/WHERE | 开篇是否交代WHO/WHERE
    first_100_chars = chapter_content[:100]
    
    protagonist_name = parsed_data.characters.protagonist.GET("name", "")
    location = parsed_data.context.GET("location", "")
    
    IF protagonist_name NOT IN first_100_chars:
        check_result.confusion_points.APPEND("Opening doesn't explain who protagonist is")
        check_result.can_follow = FALSE
    END IF
    
    IF location NOT IN first_100_chars[:200]:
        check_result.confusion_points.APPEND("Opening doesn't explain location")
        # This is not CRITICAL, just WARNING | 这个不算CRITICAL，只是WARNING
    END IF
    
    # Check 2: Do character actions have motivation | 角色动作是否有动机
    unexplained_actions = FIND_UNEXPLAINED_ACTIONS(chapter_content, parsed_data)
    
    IF LENGTH(unexplained_actions) > 2:
        check_result.confusion_points.APPEND(f"There are {LENGTH(unexplained_actions)} actions lacking motivation")
        check_result.can_follow = FALSE
    END IF
    
    # Check 3: Are scene transitions abrupt | 场景跳跃是否突兀
    scenes = DETECT_SCENES(chapter_content)
    
    FOR i = 1 TO LENGTH(scenes) - 1:
        prev_scene = scenes[i-1]
        curr_scene = scenes[i]
        
        IF IS_ABRUPT_TRANSITION(prev_scene, curr_scene):
            check_result.confusion_points.APPEND(f"Scene {i} to scene {i+1} transition is abrupt")
        END IF
    END FOR
    
    RETURN check_result
END FUNCTION

FUNCTION FIND_UNEXPLAINED_ACTIONS(text, parsed_data):
    """Find actions lacking motivation | 找到缺少动机的动作"""
    
    # Extract all action sentences | 提取所有动作句
    action_sentences = EXTRACT_ACTION_SENTENCES(text)
    
    unexplained = []
    
    FOR sentence IN action_sentences:
        # Check if context before/after has motivation explanation | 检查前后是否有动机说明
        context = GET_SENTENCE_CONTEXT(text, sentence, before=2, after=1)
        
        has_motivation = (
            CONTAINS_MOTIVATION_KEYWORDS(context) OR
            CAN_INFER_FROM_CONTEXT(context, parsed_data)
        )
        
        IF NOT has_motivation AND IS_MAJOR_ACTION(sentence):
            unexplained.APPEND(sentence)
        END IF
    END FOR
    
    RETURN unexplained
END FUNCTION
```

### 5.3 Literary Goal Achievement Check | 文学目标达成检查

```python
FUNCTION CHECK_LITERARY_GOAL_ACHIEVED(scene_content, goal, parsed_data):
    """
    Check if scene achieved literary goal (improved: heuristic rules)
    检查场景是否达成文学目标（改进版：启发式规则）
    
    Acknowledge Claude limitations | 承认Claude局限：
    - Don't pursue perfect semantic understanding | 不追求完美的语义理解
    - Use heuristic rules to check "is there evidence of attempting to achieve goal" | 用启发式规则检查"是否有尝试达成目标的证据"
    """
    
    result = {
        "success": FALSE,
        "evidence": ""
    }
    
    goal_lower = goal.LOWER()
    
    # ========== Goal Type 1: Information conveyance | 信息传达类 ==========
    IF "understand" IN goal_lower OR "know" IN goal_lower:
        # Check for explanatory content | 检查是否有解释性内容
        has_explanation = CHECK_FOR_EXPLANATION(scene_content)
        
        IF has_explanation.found:
            result.success = TRUE
            result.evidence = f"Scene contains explanatory content: {has_explanation.example}"
        ELSE:
            result.evidence = "Scene lacks explanatory content (causal words, inner monologue, etc.)"
        END IF
    
    # ========== Goal Type 2: Character personality display | 人设展示类 ==========
    ELSE IF "show" IN goal_lower OR "demonstrate" IN goal_lower:
        # Extract trait to be displayed | 提取要展示的特质
        trait = EXTRACT_TRAIT_FROM_GOAL(goal_lower)
        
        IF trait:
            # Check for matching behavior | 检查是否有匹配行为
            has_behavior = CHECK_BEHAVIOR_FOR_TRAIT(scene_content, trait, parsed_data)
            
            IF has_behavior.found:
                result.success = TRUE
                result.evidence = f"Scene has behavior demonstrating '{trait}': {has_behavior.example}"
            ELSE:
                result.evidence = f"No specific behavior demonstrating '{trait}' found in scene"
            END IF
        ELSE:
            # Can't extract trait, pass by default | 无法提取特质，默认通过
            result.success = TRUE
            result.evidence = "Goal is abstract, can't check precisely"
        END IF
    
    # ========== Goal Type 3: Plot advancement | 剧情推进类 ==========
    ELSE IF "advance" IN goal_lower:
        # Check if state changed | 检查状态是否改变
        has_change = CHECK_STATE_CHANGE(scene_content, parsed_data)
        
        IF has_change.found:
            result.success = TRUE
            result.evidence = f"Scene has state change: {has_change.description}"
        ELSE:
            result.evidence = "Scene lacks obvious state change (info/relationship/position)"
        END IF
    
    # ========== Goal Type 4: Emotional goal | 情感目标 ==========
    ELSE IF "feel" IN goal_lower OR "atmosphere" IN goal_lower:
        # Check emotion intensity | 检查情绪强度
        emotion_score = DETECT_EMOTION_INTENSITY(scene_content)
        
        IF emotion_score > 60:
            result.success = TRUE
            result.evidence = f"Scene emotion intensity sufficient ({emotion_score}/100)"
        ELSE:
            result.evidence = f"Scene emotion intensity insufficient ({emotion_score}/100)"
        END IF
    
    # ========== Default: Keyword check | 默认：关键词检查 ==========
    ELSE:
        # Extract goal keywords | 提取目标关键词
        keywords = EXTRACT_KEYWORDS(goal)
        matched = [kw FOR kw IN keywords IF kw IN scene_content]
        
        IF LENGTH(matched) >= LENGTH(keywords) * 0.6:  # 60% keyword match | 60%关键词匹配
            result.success = TRUE
            result.evidence = f"Keyword match rate {LENGTH(matched)/LENGTH(keywords)*100:.0f}%"
        ELSE:
            result.evidence = f"Insufficient keyword match ({LENGTH(matched)}/{LENGTH(keywords)})"
        END IF
    END IF
    
    RETURN result
END FUNCTION

# ========== Helper Check Functions | 辅助检查函数 ==========

FUNCTION CHECK_FOR_EXPLANATION(text):
    """Check for explanatory content | 检查是否有解释性内容"""
    
    # Causal words | 因果词
    causal_words = ["because", "so", "in order to", "caused", "the reason is"]
    
    FOR word IN causal_words:
        IF word IN text:
            # Extract sentence containing this word as example | 提取包含该词的句子作为示例
            example = EXTRACT_SENTENCE_WITH_WORD(text, word)
            RETURN {"found": TRUE, "example": example[:50]}
        END IF
    END FOR
    
    # Inner monologue (usually for explaining motivation) | 内心独白（通常用于解释动机）
    IF "he thought" IN text OR "she thought" IN text OR "in mind" IN text:
        example = EXTRACT_INNER_MONOLOGUE(text)
        IF LENGTH(example) > 10:
            RETURN {"found": TRUE, "example": example[:50]}
        END IF
    END IF
    
    RETURN {"found": FALSE, "example": ""}
END FUNCTION

FUNCTION EXTRACT_TRAIT_FROM_GOAL(goal):
    """Extract personality trait from goal | 从目标中提取人设特质"""
    
    # Common personality traits | 常见人设特质
    traits = ["cautious", "impulsive", "greedy", "kind", "cunning", "brave", "cowardly", "proud", "humble"]
    
    FOR trait IN traits:
        IF trait IN goal:
            RETURN trait
        END IF
    END FOR
    
    # If no direct trait words, try to infer | 如果没有直接特质词，尝试推断
    IF "careful" IN goal OR "vigilant" IN goal:
        RETURN "cautious"
    ELSE IF "decisive" IN goal OR "not hesitate" IN goal:
        RETURN "impulsive"
    END IF
    
    RETURN NULL
END FUNCTION

FUNCTION CHECK_BEHAVIOR_FOR_TRAIT(text, trait, parsed_data):
    """Check for behavior demonstrating trait | 检查是否有体现特质的行为"""
    
    protagonist_name = parsed_data.characters.protagonist.GET("name", "protagonist")
    
    # Find matching behavior based on trait | 根据特质查找匹配行为
    SWITCH trait:
        CASE "cautious":
            keywords = ["observe", "wait", "probe", "retreat", "confirm", "careful"]
        
        CASE "impulsive":
            keywords = ["immediately", "directly", "didn't think", "can't worry about", "rush"]
        
        CASE "greedy":
            keywords = ["want", "all", "more", "not enough", "eyes lit up"]
        
        CASE "kind":
            keywords = ["help", "save", "pity", "can't bear", "care about"]
        
        DEFAULT:
            keywords = [trait]  # Default to searching for trait word itself | 默认查找特质词本身
    END SWITCH
    
    # Find matching behavior performed by protagonist | 查找主角执行的匹配行为
    FOR keyword IN keywords:
        sentences = SPLIT_SENTENCES(text)
        
        FOR sentence IN sentences:
            IF protagonist_name IN sentence AND keyword IN sentence:
                RETURN {"found": TRUE, "example": sentence[:50]}
            END IF
        END FOR
    END FOR
    
    RETURN {"found": FALSE, "example": ""}
END FUNCTION

FUNCTION CHECK_STATE_CHANGE(text, parsed_data):
    """Check for state change | 检查是否有状态变化"""
    
    changes = []
    
    # Check 1: Info change (discover new info) | 信息变化（发现新信息）
    info_keywords = ["discover", "know", "learn", "hear", "see"]
    FOR keyword IN info_keywords:
        IF keyword IN text:
            changes.APPEND(f"Info change ({keyword})")
            BREAK
        END IF
    END FOR
    
    # Check 2: Relationship change | 关系变化
    relationship_keywords = ["become", "break", "reconcile", "trust", "suspect"]
    FOR keyword IN relationship_keywords:
        IF keyword IN text:
            changes.APPEND(f"Relationship change ({keyword})")
            BREAK
        END IF
    END FOR
    
    # Check 3: Position change | 位置变化
    location_keywords = ["arrive at", "leave", "enter", "exit", "reach"]
    FOR keyword IN location_keywords:
        IF keyword IN text:
            changes.APPEND(f"Position change ({keyword})")
            BREAK
        END IF
    END FOR
    
    # Check 4: Item change | 物品变化
    item_keywords = ["get", "lose", "pick up", "lost", "obtain"]
    FOR keyword IN item_keywords:
        IF keyword IN text:
            changes.APPEND(f"Item change ({keyword})")
            BREAK
        END IF
    END FOR
    
    IF LENGTH(changes) > 0:
        RETURN {"found": TRUE, "description": JOIN(changes, ", ")}
    ELSE:
        RETURN {"found": FALSE, "description": ""}
    END IF
END FUNCTION
```

---

## §6 Intelligent Fix System | Problem Tracing and Rewrite
## §6 智能修复系统 | 问题溯源与重写

### 6.1 Fix Decision Tree | 修复决策树

```python
FUNCTION DECIDE_FIX_OR_REWRITE(diagnosis, chapter_content, parsed_data):
    """
    Decision: fix or rewrite
    决策：修复还是重写
    """
    
    # Rule 1: CRITICAL literary issues → MUST rewrite | 有CRITICAL文学问题 → 必须重写
    critical_issues = FILTER(diagnosis.literary_issues, lambda issue: issue.severity == "CRITICAL")
    
    IF LENGTH(critical_issues) > 0:
        # Check if problems are concentrated in few scenes | 检查问题是否集中在少数场景
        problematic_scenes = []
        FOR issue IN critical_issues:
            IF "scene_idx" IN issue:
                IF issue.scene_idx NOT IN problematic_scenes:
                    problematic_scenes.APPEND(issue.scene_idx)
                END IF
            END IF
        END FOR
        
        total_scenes = COUNT_SCENES(chapter_content)
        
        IF LENGTH(problematic_scenes) > 0 AND LENGTH(problematic_scenes) <= total_scenes * 0.3:
            # Less than 30% of scenes have problems → partial rewrite | 少于30%的场景有问题 → 部分重写
            RETURN {
                "action": "PARTIAL_REWRITE",
                "reason": f"{LENGTH(problematic_scenes)} scenes need rewriting",
                "scenes": problematic_scenes,
                "issues": critical_issues
            }
        ELSE:
            # Majority of scenes have problems or global issue → full rewrite | 多数场景有问题或全局问题 → 全部重写
            RETURN {
                "action": "FULL_REWRITE",
                "reason": f"Exists {LENGTH(critical_issues)} CRITICAL issues, wide impact range",
                "focus_areas": MAP(critical_issues, lambda i: i.type)
            }
        END IF
    END IF
    
    # Rule 2: Multiple WARNING issues → attempt fix | 多个WARNING问题 → 尝试修复
    warning_issues = FILTER(diagnosis.literary_issues, lambda issue: issue.severity == "WARNING")
    
    IF LENGTH(warning_issues) >= 3:
        RETURN {
            "action": "FIX",
            "reason": "Multiple WARNING issues exist, attempt fix",
            "fixes": diagnosis.fixes
        }
    END IF
    
    # Rule 3: Only technical metric issues → fix | 仅技术指标问题 → 修复
    IF LENGTH(critical_issues) == 0 AND LENGTH(warning_issues) <= 2:
        IF diagnosis.technical_metrics.word_count < 1800:
            RETURN {
                "action": "FIX",
                "reason": "Word count slightly low, expand key scenes",
                "fixes": ["expand_scenes"]
            }
        END IF
    END IF
    
    # Default: pass | 默认：通过
    RETURN {
        "action": "PASS",
        "reason": "No fix needed"
    }
END FUNCTION

FUNCTION COUNT_SCENES(text):
    """Count number of scenes in chapter (simplified implementation) | 统计章节中的场景数量（简化实现）"""
    
    # Method: segment by blank lines, paragraph count approximates scene count | 方法：以空行分段，段数近似场景数
    paragraphs = SPLIT_PARAGRAPHS(text)
    
    # Assume every 3-5 paragraphs is one scene | 假设每3-5段为一个场景
    estimated_scenes = MAX(1, ROUND(LENGTH(paragraphs) / 4))
    
    RETURN estimated_scenes
END FUNCTION
```

### 6.2 Intelligent Rewrite Engine | 智能重写引擎

```python
FUNCTION INTELLIGENT_REWRITE(CAPSULE, original_plan, diagnosis):
    """
    Intelligent rewrite: targeted rewrite based on diagnosis results
    智能重写：根据诊断结果针对性重写
    
    Core concept: Not full rewrite, but locate problem scenes for rewrite
    核心理念：不是全部重写，而是定位问题场景重写
    """
    
    parsed_data = PARSE_CAPSULE(CAPSULE)
    
    # STEP 1: Analyze problem root cause | 分析问题根源
    problem_analysis = ANALYZE_PROBLEM_ROOT_CAUSE(diagnosis, original_plan)
    
    # STEP 2: Generate fix instruction | 生成修正指令
    fix_instruction = {
        "problematic_scenes": [],  # Scenes needing rewrite | 需要重写的场景
        "adjustments": {},  # Scene adjustment suggestions | 场景调整建议
        "focus": []  # Rewrite focus | 重写重点
    }
    
    FOR issue IN diagnosis.literary_issues:
        IF issue.severity == "CRITICAL":
            IF "scene_idx" IN issue:
                # Locate to specific scene | 定位到具体场景
                fix_instruction.problematic_scenes.APPEND(issue.scene_idx)
                fix_instruction.adjustments[issue.scene_idx] = {
                    "problem": issue.description,
                    "strategy": issue.fix_strategy,
                    "goal": original_plan.scenes[issue.scene_idx-1].literary_goal
                }
            ELSE:
                # Global problem | 全局问题
                fix_instruction.focus.APPEND(issue.type)
            END IF
        END IF
    END FOR
    
    # STEP 3: Revise plan | 重新规划
    revised_plan = REVISE_PLAN(original_plan, fix_instruction, parsed_data)
    
    # STEP 4: Rewrite problem scenes | 重写问题场景
    chapter_content = ""
    monitors = INIT_MONITORS()
    
    FOR scene_plan IN revised_plan.scenes:
        IF scene_plan.scene_idx IN fix_instruction.problematic_scenes:
            # Rewrite problem scene | 重写问题场景
            PRINT f"[REWRITE] Scene {scene_plan.scene_idx}: {fix_instruction.adjustments[scene_plan.scene_idx].problem}"
            
            scene_text = REWRITE_SCENE_WITH_GUIDANCE(
                scene_plan,
                fix_instruction.adjustments[scene_plan.scene_idx],
                parsed_data
            )
        ELSE:
            # Retain original scene approach | 保留原场景写法
            scene_text = WRITE_SCENE_BY_TYPE(scene_plan, parsed_data, chapter_content, monitors)
        END IF
        
        chapter_content += scene_text
        UPDATE_MONITORS(monitors, scene_text, scene_plan)
    END FOR
    
    # STEP 5: Re-diagnose | 重新诊断
    new_diagnosis = DIAGNOSE_LITERARY_QUALITY(chapter_content, parsed_data, revised_plan)
    
    # STEP 6: Deliver | 交付
    new_facts = EXTRACT_FACTS(chapter_content, parsed_data)
    RETURN DELIVER_OUTPUT_V4(chapter_content, new_facts, new_diagnosis, revised_plan)
END FUNCTION
```

### 6.3 Scene Rewrite Guidance | 场景重写指导

```python
FUNCTION REWRITE_SCENE_WITH_GUIDANCE(scene_plan, guidance, parsed_data):
    """
    Rewrite scene according to guidance
    按照指导重写场景
    """
    
    PRINT f"[REWRITE GUIDANCE] Problem: {guidance.problem}"
    PRINT f"[REWRITE GUIDANCE] Strategy: {guidance.strategy}"
    PRINT f"[REWRITE GUIDANCE] Goal: {guidance.goal}"
    
    # Select rewrite method based on strategy | 根据策略选择重写方法
    SWITCH guidance.strategy:
        CASE "REWRITE_CONFUSING_PARTS":
            # Add scene positioning and motivation explanation | 增加场景定位和动机说明
            scene_text = WRITE_SCENE_WITH_EXTRA_CLARITY(scene_plan, parsed_data)
        
        CASE "REWRITE_SCENE":
            # Complete rewrite, ensure goal achievement | 完全重写，确保达成目标
            scene_text = WRITE_SCENE_FOCUSED_ON_GOAL(scene_plan, guidance.goal, parsed_data)
        
        CASE "REWRITE_OOC_PARTS":
            # Rewrite character behavior | 重写角色行为
            scene_text = WRITE_SCENE_RESPECTING_PERSONALITY(scene_plan, parsed_data)
        
        DEFAULT:
            # Default: regular rewrite | 默认：常规重写
            scene_text = WRITE_SCENE_BY_TYPE(scene_plan, parsed_data, "", INIT_MONITORS())
    END SWITCH
    
    RETURN scene_text
END FUNCTION
```

---

## §7 Lightweight Polish Layer | Handle Only Obvious Issues
## §7 轻量润色层 | 仅处理明显问题

### 7.1 Lightweight Polish Principles | 轻量润色原则

```python
FUNCTION LIGHT_POLISH(chapter_content, parsed_data):
    """
    Lightweight polish: handle only obvious issues, don't over-correct
    轻量润色：仅处理明显问题，不过度矫正
    
    Core improvement | 核心改进：
    1. Only delete obvious filler words | 只删除明显的口水词
    2. Only fix obvious emotion word tell-style | 只修复明显的情绪词直写
    3. Don't force compress inner monologue | 不强制压缩内心戏
    4. Don't force delete explanatory sentences (unless obviously redundant) | 不强制删除解释句（除非明显冗余）
    """
    
    polished = chapter_content
    
    # Polish 1: Delete obvious filler words (only high-frequency words) | 润色1：删除明显口水词（仅限高频词）
    obvious_fillers = ["seemed", "appeared", "perhaps"]
    FOR word IN obvious_fillers:
        polished = REPLACE_ALL(polished, word, "")
    END FOR
    
    # Polish 2: Intelligently rewrite "felt XX" pattern (not mechanical replacement) | 智能改写"感到XX"句式（而非机械替换）
    sentences = SPLIT_SENTENCES(polished)
    rewritten_sentences = []
    
    FOR i, sentence IN ENUMERATE(sentences):
        # Detect "felt XX" pattern | 检测"感到XX"模式
        IF MATCH(sentence, r"felt (shocked|fearful|angry)"):
            emotion = EXTRACT_EMOTION(sentence)
            
            # Get context | 获取上下文
            context_before = sentences[MAX(0, i-1):i]
            context_after = sentences[i+1:MIN(i+2, LENGTH(sentences))]
            
            # Judge: does it need rewriting? | 判断：是否需要改写？
            IF IS_REDUNDANT_EMOTION(sentence, context_before, context_after):
                # Rewrite entire sentence | 改写整句
                rewritten = REWRITE_EMOTION_SENTENCE(sentence, emotion, context_after)
                rewritten_sentences.APPEND(rewritten)
            ELSE:
                # Keep original sentence | 保留原句
                rewritten_sentences.APPEND(sentence)
            END IF
        ELSE:
            rewritten_sentences.APPEND(sentence)
        END IF
    END FOR
    
    polished = JOIN(rewritten_sentences, "")
    
    # Polish 3: Delete obvious formulaic turn words (only "however" "at this moment") | 删除明显的套路转折词（仅限"然而""就在这时"）
    polished = REPLACE_ALL(polished, "However, ", "")
    polished = REPLACE_ALL(polished, "At this moment, ", "")
    
    # Polish 4: Compress extra-long paragraphs (>200 chars) | 压缩超长段落（>200字的段落）
    paragraphs = SPLIT_PARAGRAPHS(polished)
    
    FOR i, para IN ENUMERATE(paragraphs):
        IF LENGTH(para) > 200:
            # Find appropriate split point (comma or period) | 寻找合适的分割点（逗号或句号）
            split_point = FIND_SPLIT_POINT(para, 120)
            IF split_point > 0:
                paragraphs[i] = para[:split_point] + "\n\n" + para[split_point:]
            END IF
        END IF
    END FOR
    
    polished = JOIN(paragraphs, "\n\n")
    
    RETURN CLEAN_WHITESPACE(polished)
END FUNCTION

FUNCTION EXTRACT_EMOTION(sentence):
    """Extract emotion word from sentence | 从句子中提取情绪词"""
    emotions = ["shocked", "fearful", "angry", "joyful", "sad"]
    
    FOR emotion IN emotions:
        IF emotion IN sentence:
            RETURN emotion
        END IF
    END FOR
    
    RETURN "emotion"
END FUNCTION

FUNCTION IS_REDUNDANT_EMOTION(sentence, before, after):
    """Judge if emotion sentence is redundant (later text already has explanation) | 判断情绪句是否冗余（后文已有解释）"""
    
    # If later text has "didn't expect" "how is that possible" etc. explanations, "felt shocked" is redundant | 如果后文有"没想到""怎么可能"等解释，前面的"感到震惊"就冗余
    explanation_keywords = ["didn't expect", "how is that possible", "can't believe", "unexpected"]
    
    FOR next_sent IN after:
        IF ANY(keyword IN next_sent FOR keyword IN explanation_keywords):
            RETURN TRUE  # Later text has explanation, earlier emotion word redundant | 后文有解释，前面的情绪词冗余
        END IF
    END FOR
    
    RETURN FALSE
END FUNCTION

FUNCTION REWRITE_EMOTION_SENTENCE(sentence, emotion, context_after):
    """Rewrite emotion sentence | 改写情绪句"""
    
    # Physiological response mapping | 生理反应映射
    physiology_map = {
        "shocked": "pupils suddenly contracted",
        "fearful": "back went cold",
        "angry": "temples throbbed"
    }
    
    physiology = physiology_map.GET(emotion, "heart skipped a beat")
    
    # If later text has explanation, only keep physiological response | 如果后文有解释，只保留生理反应
    IF LENGTH(context_after) > 0:
        RETURN physiology + "."
    ELSE:
        # No later explanation, keep physiological response + brief inner thought | 后文无解释，保留生理反应+简短内心
        RETURN physiology + ", how is this possible?"
    END IF
END FUNCTION
```

---

## §8 Delivery Protocol | Clear Result Presentation
## §8 交付协议 | 清晰的结果呈现

### 8.1 Delivery Format | 交付格式

```python
FUNCTION DELIVER_OUTPUT_V4(chapter_content, new_facts, diagnosis, plan):
    """
    v4.1 delivery format: highlight literary assessment + Tomato style check
    v4.1交付格式：突出文学性评估 + 番茄风格检查
    """
    
    output = {
        "chapter_content": chapter_content,
        "summary": "",
        "literary_assessment": "",
        "tomato_report": "",  # NEW | 新增
        "technical_reference": "",
        "new_facts": ""
    }
    
    # ========== Quick Summary | 快速摘要 ==========
    status = "✅ Passed" IF LENGTH(diagnosis.literary_issues) == 0 ELSE "⚠️ Issues exist"
    
    output.summary = f"""
📊 Creation Summary

**Status**: {status}
**Word Count**: {diagnosis.technical_metrics.word_count}
**Scene Count**: {LENGTH(plan.scenes)}

"""
    
    # ========== Literary Assessment (Core) | 文学性评估（核心）==========
    output.literary_assessment = "## 🎭 Literary Assessment\n\n"
    
    IF LENGTH(diagnosis.literary_issues) == 0:
        output.literary_assessment += "✅ **All scenes achieved literary goals**\n\n"
    ELSE:
        output.literary_assessment += "### ⚠️ Issues Requiring Attention\n\n"
        
        FOR issue IN diagnosis.literary_issues:
            severity_icon = "🔴" IF issue.severity == "CRITICAL" ELSE "🟡"
            output.literary_assessment += f"{severity_icon} **{issue.type}** ({issue.severity})\n"
            output.literary_assessment += f"   {issue.description}\n"
            
            IF "scene_idx" IN issue:
                output.literary_assessment += f"   Scene: {issue.scene_idx}\n"
            END IF
            
            IF "fix_strategy" IN issue:
                output.literary_assessment += f"   Suggestion: {issue.fix_strategy}\n"
            END IF
            
            output.literary_assessment += "\n"
        END FOR
    END IF
    
    # Reader experience assessment | 读者体验评估
    IF "reader_experience" IN diagnosis:
        output.literary_assessment += "### 📖 Reader Experience Prediction\n\n"
        output.literary_assessment += f"- Comprehension: {'✅ Clear' IF diagnosis.reader_experience.can_follow ELSE '❌ May be confused'}\n"
        output.literary_assessment += f"- Visual Imagery: {diagnosis.reader_experience.GET('imagery_score', 0)}/10\n"
        output.literary_assessment += f"- Emotion Curve: {'✅ Has ups and downs' IF NOT diagnosis.reader_experience.GET('emotion_flat', TRUE) ELSE '⚠️ Flat'}\n\n"
    END IF
    
    # ========== NEW: Tomato Style Report | 番茄风格报告 ==========
    output.tomato_report = "## 🍅 Tomato Style Check\n\n"
    
    # Cool point delivery | 爽点交付
    cool_check = CHECK_COOL_POINT_DELIVERY(chapter_content, plan)
    output.tomato_report += f"### 💥 Cool Factor Score\n"
    output.tomato_report += f"**{cool_check.coolness_score}/10** "
    
    IF cool_check.coolness_score >= 7:
        output.tomato_report += "✅ Sufficient cool factor\n\n"
    ELSE IF cool_check.coolness_score >= 5:
        output.tomato_report += "⚠️ Average cool factor\n\n"
    ELSE:
        output.tomato_report += "❌ Insufficient cool factor\n\n"
    END IF
    
    IF LENGTH(cool_check.missing_cool_points) > 0:
        output.tomato_report += "**Missing Cool Points**:\n"
        FOR missing IN cool_check.missing_cool_points:
            output.tomato_report += f"  - Scene {missing.scene_idx}: {missing.cool_type}\n"
        END FOR
        output.tomato_report += "\n"
    ELSE:
        output.tomato_report += "✅ All planned cool points delivered\n\n"
    END IF
    
    # Hook density | 钩子密度
    hook_count = COUNT_HOOKS_IN_TEXT(chapter_content)
    total_length = diagnosis.technical_metrics.word_count
    avg_interval = total_length / MAX(hook_count, 1)
    hook_status = "✅" IF avg_interval <= 600 ELSE "⚠️"
    
    output.tomato_report += f"### 🎣 Hook Density\n"
    output.tomato_report += f"**{hook_count} hooks** / {total_length} chars\n"
    output.tomato_report += f"{hook_status} Average {avg_interval:.0f} chars per hook (standard ≤600 chars)\n\n"
    
    IF avg_interval > 600:
        output.tomato_report += f"⚠️ Hook interval too large, suggest adding micro-conflicts or small discoveries in boring stretches\n\n"
    END IF
    
    # Tomato core metrics | 番茄核心指标
    # Tomato style description | 番茄特色说明
    output.tomato_report += "\n> 💡 **Tomato Style Features**: Dense cool points, frequent hooks, tight pacing, high information density\n\n"
    output.tomato_report += "### 📊 Tomato Core Metrics\n\n"
    output.tomato_report += "| Metric | Current | Standard Range | Status |\n"
    output.tomato_report += "|--------|---------|----------------|--------|\n"
    
    # Metric 1: Dialogue ratio | 对话占比
    dialogue_ratio = diagnosis.technical_metrics.dialogue_ratio
    dialogue_status = "✅" IF 0.30 <= dialogue_ratio <= 0.50 ELSE "⚠️"
    output.tomato_report += f"| Dialogue Ratio | {dialogue_ratio*100:.1f}% | 30-50% | {dialogue_status} |\n"
    
    # Metric 2: Info density | 信息密度
    info_density = diagnosis.technical_metrics.info_density
    density_status = "✅" IF info_density >= 0.01 ELSE "⚠️"
    output.tomato_report += f"| Info Density | {info_density:.3f} | ≥0.01 | {density_status} |\n"
    
    # Metric 3: Average paragraph length | 平均段落长度
    avg_para = diagnosis.technical_metrics.avg_para_length
    para_status = "✅" IF avg_para <= 150 ELSE "⚠️"
    output.tomato_report += f"| Avg Paragraph | {avg_para:.0f} chars | ≤150 chars | {para_status} |\n"
    
    output.tomato_report += "\n"
    
    # Tomato style suggestions | 番茄风格建议
    tomato_issues = FILTER(diagnosis.literary_issues, lambda i: i.type == "TOMATO_STYLE")
    
    IF LENGTH(tomato_issues) > 0:
        output.tomato_report += "### ⚠️ Tomato Style Issues\n\n"
        FOR issue IN tomato_issues:
            output.tomato_report += f"- {issue.description}\n"
            IF "fix_strategy" IN issue:
                output.tomato_report += f"  💡 {issue.fix_strategy}\n"
            END IF
        END FOR
        output.tomato_report += "\n"
    END IF
    
    # ========== Technical Metrics (Reference) | 技术指标（参考）==========
    metrics = diagnosis.technical_metrics
    
    output.technical_reference = f"""
## 📐 Technical Metrics (Reference)

| Metric | Current | Reference Range | Status |
|--------|---------|-----------------|--------|
| Word Count | {metrics.word_count} | 2000-6000 | {'✅' IF 2000 <= metrics.word_count <= 6000 ELSE '⚠️'} |
| Dialogue Ratio | {metrics.dialogue_ratio*100:.1f}% | 30-50% | {'✅' IF 0.30 <= metrics.dialogue_ratio <= 0.50 ELSE '📊'} |
| Info Density | {metrics.info_density:.3f} | ≥0.01 | {'✅' IF metrics.info_density >= 0.01 ELSE '⚠️'} |
| Avg Paragraph | {metrics.avg_para_length:.0f} chars | ≤150 chars | {'✅' IF metrics.avg_para_length <= 150 ELSE '📏'} |

**Note**: Technical metrics are for reference only, not the sole standard for quality judgment.
"""
    
    # ========== New Facts | 新增Fact ==========
    output.new_facts = FORMAT_FACTS_LIST(new_facts)
    
    # ========== Scene Type Report | 场景类型报告 ==========
    scene_type_report = "\n## 🎬 Scene Type Distribution\n\n"
    
    FOR scene_plan IN plan.scenes:
        scene_type_report += f"**Scene {scene_plan.scene_idx}** ({scene_plan.type})\n"
        scene_type_report += f"- Goal: {scene_plan.literary_goal}\n"
        scene_type_report += f"- Word Count: {scene_plan.GET('actual_words', scene_plan.budget)} chars\n"
        
        # Show cool point (if any) | 显示爽点（如果有）
        IF scene_plan.scene_idx IN plan.GET("cool_point_plan", {}):
            cool_type = plan.cool_point_plan[scene_plan.scene_idx]
            output_icon = "💥"
            scene_type_report += f"- Cool Point: {output_icon} {cool_type}\n"
        END IF
        
        scene_type_report += "\n"
    END FOR
    
    output.summary += scene_type_report
    
    # ========== Assemble Final Output | 组装最终输出 ==========
    final_output = (
        output.summary + "\n" + 
        output.literary_assessment + "\n" + 
        output.tomato_report + "\n" +  # NEW Tomato report | 新增番茄报告
        output.technical_reference + "\n" + 
        output.new_facts
    )
    
    RETURN {
        "content": chapter_content,
        "report": final_output
    }
END FUNCTION
```

---

## §9 Tool Function Library (Simplified Version)
## §9 工具函数库（精简版）

### 9.1 Core Check Functions | 核心检查函数

```python
# ==================== Dialogue Quality Check | 对话质量检查 ====================

FUNCTION IS_GOOD_DIALOGUE(dialogue):
    """Judge if dialogue is effective (refer to §0.3 standard) | 判断对话是否有效（参考§0.3标准）"""
    
    # Check 1: Does it advance plot | 检查1：是否推进剧情
    plot_advancing_keywords = ["discover", "get", "know", "decide", "go", "come", "walk"]
    IF ANY(keyword IN dialogue.content FOR keyword IN plot_advancing_keywords):
        RETURN TRUE
    END IF
    
    # Check 2: Does it reveal character | 检查2：是否揭示角色
    IF LENGTH(dialogue.content) > 15:  # Substantial dialogue | 有实质内容的对话
        RETURN TRUE
    END IF
    
    # Check 3: Does it create tension | 检查3：是否制造张力
    tension_keywords = ["but", "not", "why", "how", "could it be"]
    IF ANY(keyword IN dialogue.content FOR keyword IN tension_keywords):
        RETURN TRUE
    END IF
    
    # Otherwise: ineffective dialogue (like "um" "ah" "oh" or repetitive info) | 否则：无效对话（如"嗯""啊""哦"或重复信息）
    RETURN FALSE
END FUNCTION

# ==================== Visual Imagery Scoring | 画面感评分 ====================

FUNCTION CALCULATE_IMAGERY_SCORE(text):
    """Calculate visual imagery score (0-10) | 计算画面感评分（0-10）"""
    
    score = 5.0  # Baseline score | 基准分
    
    # Plus point 1: Sensory words | 加分项1：感官词汇
    sensory_words = ["saw", "heard", "smelled", "touched", "tasted", "icy", "warm", "pungent", "soft"]
    sensory_count = SUM([COUNT_OCCURRENCES(text, word) FOR word IN sensory_words])
    score += MIN(sensory_count * 0.2, 2.0)
    
    # Plus point 2: Concrete nouns (not abstract words) | 加分项2：具体名词（而非抽象词）
    concrete_nouns = COUNT_CONCRETE_NOUNS(text)
    abstract_nouns = COUNT_ABSTRACT_NOUNS(text)
    
    IF concrete_nouns > abstract_nouns:
        score += 1.5
    ELSE IF concrete_nouns < abstract_nouns * 0.5:
        score -= 1.5
    END IF
    
    # Minus point: Too many "seemed" "appeared" (residual abstract description) | 扣分项：过多的"似乎""仿佛"（残留的抽象描述）
    abstract_markers = COUNT_OCCURRENCES(text, "seemed") + COUNT_OCCURRENCES(text, "appeared")
    score -= abstract_markers * 0.3
    
    RETURN CLAMP(score, 0, 10)
END FUNCTION

# ==================== Emotion Curve Analysis | 情绪曲线分析 ====================

FUNCTION ANALYZE_EMOTION_FLOW(text, parsed_data):
    """Analyze emotion curve | 分析情绪曲线"""
    
    # Analyze emotion by paragraph | 分段分析情绪
    paragraphs = SPLIT_PARAGRAPHS(text)
    emotion_points = []
    
    FOR para IN paragraphs:
        intensity = DETECT_EMOTION_INTENSITY(para)
        emotion_points.APPEND(intensity)
    END FOR
    
    # Check for ups and downs | 检查是否有起伏
    max_intensity = MAX(emotion_points)
    min_intensity = MIN(emotion_points)
    
    is_flat = (max_intensity - min_intensity) < 20  # Difference <20 considered flat | 差值<20认为平淡
    
    RETURN {
        "emotion_points": emotion_points,
        "is_flat": is_flat,
        "max": max_intensity,
        "min": min_intensity
    }
END FUNCTION

FUNCTION DETECT_EMOTION_INTENSITY(para):
    """Detect paragraph emotion intensity (0-100) | 检测段落情绪强度（0-100）"""
    
    intensity = 50  # Baseline | 基准
    
    # Physiological response words (strong emotion markers) | 生理反应词（强烈情绪标志）
    physiology_keywords = ["heartbeat", "breath", "cold sweat", "pupils", "stomach", "tremble", "froze"]
    physiology_count = SUM([COUNT_OCCURRENCES(para, word) FOR word IN physiology_keywords])
    intensity += physiology_count * 10
    
    # Action speed words (tension markers) | 动作速度词（紧张感标志）
    speed_keywords = ["fiercely", "suddenly", "instantly", "immediately", "right away"]
    speed_count = SUM([COUNT_OCCURRENCES(para, word) FOR word IN speed_keywords])
    intensity += speed_count * 8
    
    # Reversal words in dialogue (conflict markers) | 对话中的反转词（冲突标志）
    conflict_keywords = ["but", "not", "why", "what right", "dream on"]
    conflict_count = SUM([COUNT_OCCURRENCES(para, word) FOR word IN conflict_keywords])
    intensity += conflict_count * 5
    
    RETURN CLAMP(intensity, 0, 100)
END FUNCTION
```

### 9.2 Helper Generation Functions | 辅助生成函数

```python
FUNCTION GENERATE_SENSORY_DETAILS(item, context):
    """Generate sensory details (extract from §13 or generate) | 生成感官细节（从§13提取或生成）"""
    # Refer to v3.1 implementation, simplified here | 参考v3.1实现，此处简化
    RETURN f"{item.name}'s detailed description."
END FUNCTION

FUNCTION GENERATE_CONTEXTUAL_DIALOGUE(text, insertion_point, parsed_data, target_chars):
    """Generate context-appropriate dialogue | 生成符合上下文的对话"""
    # Generate reasonable dialogue based on context | 根据上下文生成合理的对话
    context_before = text[MAX(0, insertion_point-200):insertion_point]
    
    # Extract topic mentioned in previous text | 提取前文提到的话题
    topic = EXTRACT_TOPIC_FROM_CONTEXT(context_before)
    
    # Generate related dialogue | 生成相关对话
    characters = EXTRACT_NEARBY_CHARACTERS(context_before, parsed_data)
    
    IF LENGTH(characters) >= 2:
        dialogue = f""{characters[0].name} asked a question."\n\n"{characters[1].name} responded.""
        RETURN dialogue
    ELSE:
        RETURN ""
    END IF
END FUNCTION
```

---

## §10 Execution Example | 执行示例

```python
FUNCTION MAIN():
    """Main program entry | 主程序入口"""
    
    PRINT """
╔══════════════════════════════════════════════════════════╗
║   Claude Writing System v4.0 - Scene Type Driven      ║
║   & Literary Quality Priority                           ║
║   Claude写作系统v4.0 - 场景类型驱动 & 文学性优先      ║
╚══════════════════════════════════════════════════════════╝
"""
    
    # Load capsule | 加载胶囊
    TRY:
        CAPSULE = READ_FILE("Capsule.md")
        PRINT "[✓] Information capsule loaded successfully\n"
    CATCH FileNotFoundError:
        PRINT "[✗] Capsule.md file not found"
        RETURN
    END TRY
    
    # Execute main process | 执行主流程
    TRY:
        result = MAIN_EXECUTION_V4(CAPSULE)
        
        # Output results | 输出结果
        PRINT "\n" + "="*60
        PRINT result.report
        PRINT "="*60 + "\n"
        
        PRINT "## 📝 Chapter Content\n"
        PRINT result.content
        
    CATCH Exception AS e:
        PRINT f"[✗] Execution failed: {e.message}"
        PRINT f"Suggestion: {GET_ERROR_SUGGESTION(e)}"
    END TRY
    
    PRINT "\n[✓] System execution complete"
END FUNCTION

IF __name__ == "__main__":
    MAIN()
END IF
```

---

## 📋 Appendix: Quick Reference Card
## 📋 附录：快速参考卡

### A1. Scene Type Quick Reference | 场景类型速查

| Type | Dialogue | Inner Monologue | Focus |
|------|----------|-----------------|-------|
| solo_exploration | 10-25% | 20-35% | Visual imagery, discovery process |
| two_person_dialogue | 40-60% | 5-15% | Dialogue advances plot |
| group_conflict | 45-65% | 0-10% | Fast pace, conflict escalation |
| crisis_response | 15-30% | 5-15% | Physiological response, tension |
| emotional_turning_point | 25-40% | 15-30% | Emotion progression, turning point |

### A2. Constraint Priority | 约束优先级

```
P0 - RED_LINE (immediate stop | 立即中止)
  ├─ OOC
  ├─ World-building contradiction | 世界观矛盾
  └─ Logic bug | 逻辑BUG

P1 - LITERARY_GOAL (rewrite scene | 重写场景)
  ├─ Core mission incomplete | 核心任务未完成
  ├─ Unclear character motivation | 角色动机不明
  └─ Reader will be confused | 读者会困惑

P2 - QUALITY_BASELINE (attempt fix | 尝试修复)
  ├─ Word count severely low | 字数严重不足
  ├─ Info density too low | 信息密度过低
  └─ Poor dialogue quality | 对话质量差

P3 - POLISH_SUGGESTION (log warning | 记录警告)
  ├─ Dialogue ratio slightly low | 对话占比偏低
  ├─ Paragraph too long | 段落过长
  └─ Insufficient visual imagery | 画面感不足
```

### A3. Three Standards for Good Dialogue | 好对话的三个标准

1. **Advance Plot**: Reveal new info, change relationship, trigger action
2. **Reveal Character**: Show personality, show conflict, show status
3. **Create Tension**: Cause misunderstanding, escalate conflict, plant seed


---

**END OF SOP v4.0**

**Core Design Philosophy Summary**：
- Literary quality priority, technical metrics serve narration
- Scene type driven, dynamically adjust constraints
- Pre-diagnosis mechanism, avoid rework
- Intelligent fix system, locate problems for precise rewrite
- Reader experience centered, not mechanical compliance
