# 📋 Claude写作执行SOP v4.0 - 场景类型驱动系统

**设计哲学**：文学性优先，技术指标服务于叙事目标。

---

## §0 核心写作哲学 | 什么是好的小说

### 0.1 核心原则

```python
# 优先级定义
PRIORITY_ORDER = [
    "读者能否理解故事",      # P0 - 最高优先级
    "场景是否达成文学目标",   # P1
    "角色行为是否合理",       # P2
    "技术指标是否达标"        # P3 - 最低优先级
]

# 当技术指标与文学性冲突时
IF technical_metric_fails BUT literary_goal_achieved:
    ACCEPT_AND_DELIVER()  # 文学性优先
ELSE IF technical_metric_passes BUT literary_goal_fails:
    REWRITE()  # 技术指标不是目的
END IF
```

### 0.2 什么是好场景

好场景的三个标准：
1. **读者不困惑**：知道WHO/WHERE/WHY NOW，理解角色动机
2. **有东西发生**：信息/关系/情绪至少有一样在变化
3. **留下印象**：具体的画面、对话或情绪，而非抽象概念

### 0.3 什么是好对话

```python
FUNCTION IS_GOOD_DIALOGUE(dialogue):
    """好对话的判断标准"""
    
    # 检查1：是否推进剧情
    IF dialogue reveals_new_info OR changes_relationship OR triggers_action:
        RETURN TRUE
    END IF
    
    # 检查2：是否揭示角色
    IF dialogue shows_personality OR shows_conflict OR shows_status:
        RETURN TRUE
    END IF
    
    # 检查3：是否制造张力
    IF dialogue creates_misunderstanding OR escalates_conflict OR plants_seed:
        RETURN TRUE
    END IF
    
    # 否则是无效对话
    RETURN FALSE
END FUNCTION
```

### 0.4 约束层级

```python
CONSTRAINT_LEVELS = {
    "RED_LINE": {
        "priority": 0,
        "action_on_fail": "IMMEDIATE_STOP",
        "examples": ["OOC", "世界观矛盾", "逻辑BUG"]
    },
    
    "LITERARY_GOAL": {
        "priority": 1,
        "action_on_fail": "REWRITE_SCENE",
        "examples": ["核心任务未完成", "角色动机不明"]
    },
    
    "QUALITY_BASELINE": {
        "priority": 2,
        "action_on_fail": "TRY_FIX",
        "examples": ["字数严重不足", "信息密度过低"]
    },
    
    "POLISH_SUGGESTION": {
        "priority": 3,
        "action_on_fail": "LOG_WARNING",
        "examples": ["对话占比偏低", "段落过长"]
    }
}
```

---

## §1 场景类型系统 | 不同场景，不同写法

### 1.1 场景类型定义

```python
SCENE_TYPES = {
    "独处探索": {
        "特征": ["主角单独行动", "发现新信息", "思考决策"],
        "对话范围": [0.10, 0.25],  # 允许低对话
        "内心独白范围": [0.20, 0.35],  # 允许高内心戏
        "写作重点": "画面感、发现过程、思考逻辑",
        "典型结构": "触发 → 观察 → 思考 → 决策",
        
        # ========== 新增：番茄约束 ==========
        "tomato_constraints": {
            "hook_interval": 500,  # 每500字一个小钩子（独处可以稍长）
            "min_info_per_100": 1,  # 最低信息密度
            "max_boring_stretch": 400,  # 最多400字无新信息
            "must_have_discovery": TRUE,  # 必须有发现/进展
            "pace": "中速"  # 节奏：慢速/中速/快速
        }
		
		
    },
    
    "双人对话": {
        "特征": ["两个角色", "信息交换", "关系变化"],
        "对话范围": [0.40, 0.60],
        "内心独白范围": [0.05, 0.15],
        "写作重点": "对话推进剧情、揭示关系动态",
        "典型结构": "开场 → 试探 → 交锋 → 结论",
        
        # ========== 新增：番茄约束 ==========
        "tomato_constraints": {
            "hook_interval": 400,  # 每400字一个小钩子（对话更紧凑）
            "min_info_per_100": 1.5,  # 对话场景信息密度更高
            "max_boring_stretch": 300,  # 对话不能拖沓
            "must_have_tension": TRUE,  # 必须有张力（试探/冲突/反转）
            "pace": "中速"
        }
    },
    
    "群戏冲突": {
        "特征": ["3+角色", "立场分歧", "多方博弈"],
        "对话范围": [0.45, 0.65],
        "内心独白范围": [0.00, 0.10],
        "写作重点": "快节奏、多方立场、冲突升级",
        "典型结构": "引爆点 → 站队 → 交锋 → 暂时结果",
        
        # ========== 新增：番茄约束 ==========
        "tomato_constraints": {
            "hook_interval": 300,  # 每300字一个冲突点
            "min_info_per_100": 2,  # 高密度信息
            "max_boring_stretch": 200,  # 绝不拖沓
            "must_have_escalation": TRUE,  # 必须有升级
            "pace": "快速"
        }
    },
    
    "危机反应": {
        "特征": ["紧急情况", "本能反应", "生死攸关"],
        "对话范围": [0.15, 0.30],  # 危机时话少
        "内心独白范围": [0.05, 0.15],
        "写作重点": "生理反应、本能行动、紧张感",
        "典型结构": "危机 → 生理反应 → 应对行动 → 暂时安全",
        
        # ========== 新增：番茄约束 ==========
        "tomato_constraints": {
            "hook_interval": 250,  # 每250字一个危机点
            "min_info_per_100": 1.5,  # 快节奏下的高密度
            "max_boring_stretch": 150,  # 极短
            "must_have_physiology": TRUE,  # 必须有生理反应
            "pace": "快速"
        }
    },
    
    "情感转折": {
        "特征": ["情绪剧变", "认知颠覆", "关系质变"],
        "对话范围": [0.25, 0.40],
        "内心独白范围": [0.15, 0.30],
        "写作重点": "情绪递进、认知冲突、转折点",
        "典型结构": "触发 → 抗拒 → 崩溃 → 接受",
        
        # ========== 新增：番茄约束 ==========
        "tomato_constraints": {
            "hook_interval": 400,  # 每400字一个情绪点
            "min_info_per_100": 1.2,  # 中等密度
            "max_boring_stretch": 350,
            "must_have_turn": TRUE,  # 必须有情绪转折
            "pace": "中速"
        }
    }
}
```

### 1.2 场景类型识别

```python

FUNCTION IDENTIFY_SCENE_TYPE(scene_description, parsed_data):
    """
    根据场景描述自动识别类型
    
    识别逻辑（改进版）：
    1. 提取场景核心动词和目标
    2. 角色数量 + 场景目标 → 综合判断
    3. 特殊场景优先识别（危机、情感转折）
    """
    
    # 提取基础信息
    character_count = COUNT_CHARACTERS_IN_SCENE(scene_description)
    core_verbs = EXTRACT_CORE_VERBS(scene_description)
    scene_goal = scene_description.LOWER()  # 简化：直接用描述文本
    
    # ========== 优先级1：危机场景识别 ==========
    crisis_keywords = ["危机", "攻击", "逃跑", "战斗", "受伤", "追杀", "妖化"]
    IF ANY(keyword IN scene_goal FOR keyword IN crisis_keywords):
        RETURN "危机反应"
    END IF
    
    # ========== 优先级2：情感转折识别 ==========
    emotion_keywords = ["崩溃", "决裂", "醒悟", "认知", "领悟", "接受", "放弃"]
    emotion_intensity = GET_EMOTION_INTENSITY(scene_description, parsed_data)
    
    IF ANY(keyword IN scene_goal FOR keyword IN emotion_keywords) OR emotion_intensity > 70:
        RETURN "情感转折"
    END IF
    
    # ========== 优先级3：根据角色数量初步分类 ==========
    IF character_count == 1:
        # 单角色场景：检查是否有发现/探索
        discovery_keywords = ["发现", "找到", "看到", "注意到", "捡到", "得到"]
        
        IF ANY(keyword IN core_verbs FOR keyword IN discovery_keywords):
            RETURN "独处探索"
        ELSE:
            # 没有明显发现：检查是否是修炼/思考类（低张力）
            routine_keywords = ["修炼", "打坐", "休息", "整理", "回忆"]
            
            IF ANY(keyword IN scene_goal FOR keyword IN routine_keywords):
                PRINT "[WARN] 场景似乎是低张力日常，建议压缩或合并到其他场景"
                RETURN "独处探索"  # 默认归类
            ELSE:
                RETURN "独处探索"
            END IF
        END IF
    
    ELSE IF character_count == 2:
        # 双角色场景：检查是否有冲突
        conflict_keywords = ["冲突", "争执", "质问", "拒绝", "对抗", "试探"]
        
        IF ANY(keyword IN scene_goal FOR keyword IN conflict_keywords):
            RETURN "双人对话"  # 有冲突的双人戏
        ELSE:
            # 无冲突：检查是否是信息交换
            info_keywords = ["询问", "告诉", "解释", "交代", "商量"]
            
            IF ANY(keyword IN scene_goal FOR keyword IN info_keywords):
                RETURN "双人对话"
            ELSE:
                # 都不是：默认双人对话
                RETURN "双人对话"
            END IF
        END IF
    
    ELSE:  # 3+角色
        # 多角色场景：检查是否是群戏冲突
        group_conflict_keywords = ["争论", "站队", "对峙", "多方", "围攻"]
        
        IF ANY(keyword IN scene_goal FOR keyword IN group_conflict_keywords):
            RETURN "群戏冲突"
        ELSE:
            # 不是冲突：可能是群体对话（降级为双人对话）
            PRINT f"[INFO] 场景有{character_count}个角色，但无明显冲突，归类为双人对话"
            RETURN "双人对话"
        END IF
    END IF
END FUNCTION

FUNCTION EXTRACT_CORE_VERBS(text):
    """提取场景描述中的核心动词"""
    
    # 简化实现：提取常见动词
    common_verbs = [
        "发现", "找到", "看到", "遇到", "得到", "失去",
        "询问", "告诉", "解释", "争论", "拒绝", "同意",
        "攻击", "逃跑", "战斗", "修炼", "思考", "决定"
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

## §2 场景规划引擎 | 从胶囊到写作计划

### 2.1 胶囊解析（保留v3.1逻辑，简化代码）

```python
FUNCTION PARSE_CAPSULE(CAPSULE):
    """解析信息胶囊（参考v3.1实现，此处简化）"""
    parsed = EXTRACT_ALL_SECTIONS(CAPSULE)  # 提取§1-§19
    VALIDATE_REQUIRED_FIELDS(parsed)  # 校验必填项
    RETURN parsed
END FUNCTION
```

### 2.2 场景规划流程

```python
FUNCTION PLAN_CHAPTER(parsed_data):
    """
    章节规划：从胶囊生成场景计划
    
    核心改进：
    - 不再生成"任务清单"
    - 改为生成"文学目标+场景类型+约束范围"
    """
    
    plan = {
        "scenes": [],
        "total_budget": parsed_data.meta.GET("word_count_target", 2000)
    }
    
    # STEP 1: 推断场景数量和边界
    scene_boundaries = INFER_SCENE_BOUNDARIES(parsed_data)
    
    # STEP 2: 为每个场景设定目标
    FOR scene_idx, boundary IN ENUMERATE(scene_boundaries):
        scene_plan = {
            "scene_idx": scene_idx + 1,
            "type": NULL,  # 待识别
            "literary_goal": "",  # 文学目标
            "constraints": {},  # 动态约束
            "budget": 0,  # 字数预算
            "must_include": [],  # 必须包含的元素
            "avoid": []  # 必须避免的元素
        }
        
        # 2.1 识别场景类型
        scene_plan.type = IDENTIFY_SCENE_TYPE(boundary.description, parsed_data)
        
        # 2.2 设定文学目标
        scene_plan.literary_goal = EXTRACT_LITERARY_GOAL(boundary, parsed_data)
        
        # 2.3 生成动态约束
        scene_plan.constraints = GENERATE_DYNAMIC_CONSTRAINTS(
            scene_plan.type, 
            parsed_data
        )
        
        # 2.4 分配字数预算
        scene_plan.budget = ALLOCATE_SCENE_BUDGET(
            scene_plan, 
            plan.total_budget,
            scene_boundaries
        )
        
        # 2.5 提取必须/禁止元素
        scene_plan.must_include = EXTRACT_MUST_ELEMENTS(boundary, parsed_data)
        scene_plan.avoid = EXTRACT_AVOID_ELEMENTS(boundary, parsed_data)
        
        plan.scenes.APPEND(scene_plan)
    END FOR
	
	
	    # ========== 新增：爽点规划（番茄必备）==========
		total_words = plan.total_budget
		
		# 不再按字数计算爽点数量，改为分析剧情张力
		plan.cool_point_plan = PLAN_COOL_POINTS_BY_TENSION(
			plan.scenes, 
			parsed_data
		)
		
		IF LENGTH(plan.cool_point_plan) > 0:
			PRINT f"[PLAN] 根据剧情张力规划爽点数量: {LENGTH(plan.cool_point_plan)}"
			FOR scene_idx, cool_type IN plan.cool_point_plan.ITEMS():
				PRINT f"  - 场景{scene_idx}: {cool_type}"
			END FOR
		ELSE:
			PRINT "[PLAN] 本章剧情张力不足，不强制规划爽点"
		END IF
		
    
    
    RETURN plan
END FUNCTION


# ========== 新增函数 ==========


FUNCTION PLAN_COOL_POINTS_BY_TENSION(scenes, parsed_data):
    """
    根据剧情张力规划爽点（而非字数）
    
    原则：
    1. 分析每个场景的剧情张力值
    2. 在张力峰值处规划爽点
    3. 如果没有高张力场景，不强求爽点
    4. 爽点类型匹配场景类型
    """
    
    cool_plan = {}
    
    # Step 1: 计算每个场景的张力值
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
    
    # Step 2: 找到张力峰值场景（tension > 70）
    peak_scenes = FILTER(tension_scores, lambda s: s.tension > 70)
    
    IF LENGTH(peak_scenes) == 0:
        # 没有高张力场景，检查是否有中等张力（50-70）
        medium_scenes = FILTER(tension_scores, lambda s: 50 < s.tension <= 70)
        
        IF LENGTH(medium_scenes) > 0:
            # 在最高的中等张力场景安排一个爽点
            highest = MAX(medium_scenes, key=lambda s: s.tension)
            cool_plan[highest.scene_idx] = SELECT_COOL_TYPE_FOR_SCENE(highest)
            PRINT f"[COOL] 中等张力场景{highest.scene_idx}（张力{highest.tension}），安排爽点"
        ELSE:
            # 完全没有张力，不安排爽点
            PRINT "[COOL] 本章整体张力不足（<50），不安排爽点"
        END IF
        
        RETURN cool_plan
    END IF
    
    # Step 3: 在峰值场景安排爽点（最多不超过3个）
    peak_scenes = SORT(peak_scenes, key=lambda s: s.tension, reverse=TRUE)
    selected_scenes = peak_scenes[:MIN(3, LENGTH(peak_scenes))]
    
    FOR scene IN selected_scenes:
        cool_type = SELECT_COOL_TYPE_FOR_SCENE(scene)
        cool_plan[scene.scene_idx] = cool_type
        PRINT f"[COOL] 高张力场景{scene.scene_idx}（张力{scene.tension}），安排{cool_type}"
    END FOR
    
    RETURN cool_plan
END FUNCTION

FUNCTION CALCULATE_SCENE_TENSION(scene, parsed_data):
    """
    计算场景的剧情张力值（0-100）
    
    张力来源：
    - 冲突强度
    - 利益相关度
    - 情绪激烈程度
    - 时间压力
    """
    
    tension = 0
    
    # 因素1：场景类型基础张力
    type_base_tension = {
        "独处探索": 30,
        "双人对话": 40,
        "群戏冲突": 70,
        "危机反应": 85,
        "情感转折": 60
    }
    tension += type_base_tension.GET(scene.type, 30)
    
    # 因素2：场景目标中的冲突关键词
    goal = scene.literary_goal.LOWER()
    conflict_keywords = ["冲突", "对抗", "危机", "失败", "损失", "揭穿", "反转"]
    conflict_count = SUM([1 FOR keyword IN conflict_keywords IF keyword IN goal])
    tension += conflict_count * 10
    
    # 因素3：情绪强度（从胶囊§8提取）
    IF "emotions" IN parsed_data:
        emotion_intensity = parsed_data.emotions.GET("intensity", 50)
        tension += (emotion_intensity - 50) * 0.3  # 归一化影响
    END IF
    
    # 因素4：是否涉及核心任务
    IF "core_mission" IN scene.literary_goal:
        tension += 15
    END IF
    
    # 因素5：是否有"必须包含"的高张力元素
    IF "danger" IN scene.must_include:
        tension += 20
    END IF
    
    RETURN CLAMP(tension, 0, 100)
END FUNCTION

FUNCTION SELECT_COOL_TYPE_FOR_SCENE(scene):
    """根据场景类型和位置选择爽点类型"""
    
    scene_type = scene.type
    goal = scene.goal.LOWER()
    
    # 规则1：根据场景类型
    IF scene_type == "群戏冲突":
        IF "揭穿" IN goal OR "打脸" IN goal:
            RETURN "打脸爽"
        ELSE:
            RETURN "装逼爽"
        END IF
    
    ELSE IF scene_type == "危机反应":
        RETURN "反杀爽"
    
    ELSE IF scene_type == "情感转折":
        RETURN "认知爽"
    
    ELSE IF scene_type == "双人对话":
        IF "交易" IN goal OR "谈判" IN goal:
            RETURN "装逼爽"
        ELSE:
            RETURN "打脸爽"
        END IF
    
    ELSE:
        # 默认：根据剧情阶段
        IF scene.scene_idx <= 2:
            RETURN "打脸爽"
        ELSE:
            RETURN RANDOM_CHOICE(["装逼爽", "反杀爽"])
        END IF
    END IF
END FUNCTION

FUNCTION PLAN_COOL_POINTS(scenes, count, parsed_data):
    """
    规划爽点分布（番茄小说核心）
    
    规则：
    1. 优先分配到"双人对话"或"群戏冲突"场景
    2. 避免连续两个场景都是爽点（会腻）
    3. 必须在后半部分有至少一个爽点
    """
    
    cool_types = ["打脸爽", "装逼爽", "复仇爽", "升级爽", "反杀爽"]
    cool_plan = {}
    
    # 候选场景：对话/冲突类型优先
    candidates = []
    FOR i, scene IN ENUMERATE(scenes):
        IF scene.type IN ["双人对话", "群戏冲突", "情感转折"]:
            candidates.APPEND(i + 1)  # scene_idx从1开始
        END IF
    END FOR
    
    # 如果候选场景不足，补充其他场景
    IF LENGTH(candidates) < count:
        FOR i, scene IN ENUMERATE(scenes):
            IF (i + 1) NOT IN candidates:
                candidates.APPEND(i + 1)
            END IF
        END FOR
    END IF
    
    # 均匀分布爽点
    selected = DISTRIBUTE_EVENLY(candidates, count)
    
    # 分配爽点类型
    FOR i, scene_idx IN ENUMERATE(selected):
        position_ratio = scene_idx / LENGTH(scenes)
        
        IF position_ratio < 0.3:
            cool_plan[scene_idx] = RANDOM_CHOICE(["打脸爽", "装逼爽"])
        ELSE IF position_ratio < 0.7:
            cool_plan[scene_idx] = RANDOM_CHOICE(["装逼爽", "复仇爽"])
        ELSE:
            cool_plan[scene_idx] = RANDOM_CHOICE(["升级爽", "反杀爽"])
        END IF
    END FOR
    
    RETURN cool_plan
END FUNCTION

```

### 2.3 动态约束生成

```python
FUNCTION GENERATE_DYNAMIC_CONSTRAINTS(scene_type, parsed_data):
    """
    根据场景类型生成动态约束
    
    核心理念：约束是为文学目标服务的
    """
    base_constraints = SCENE_TYPES[scene_type]
    
    constraints = {
        "dialogue_ratio": base_constraints.对话范围,
        "inner_monologue_ratio": base_constraints.内心独白范围,
        "writing_focus": base_constraints.写作重点,
        "typical_structure": base_constraints.典型结构,
        
        # 动态调整部分
        "allow_low_dialogue": scene_type IN ["独处探索", "危机反应"],
        "allow_high_inner": scene_type IN ["独处探索", "情感转折"],
        "prioritize_pace": scene_type IN ["群戏冲突", "危机反应"]
    }
    
    # 根据章节整体情绪调整
    IF "emotions" IN parsed_data:
        emotion_intensity = parsed_data.emotions.GET("intensity", 50)
        
        IF emotion_intensity > 70:
            # 高情绪场景：压缩内心戏，增加行动
            constraints.inner_monologue_ratio = [
                constraints.inner_monologue_ratio[0] * 0.7,
                constraints.inner_monologue_ratio[1] * 0.7
            ]
        END IF
    END IF
    
    RETURN constraints
END FUNCTION
```

### 2.4 文学目标提取

```python
FUNCTION EXTRACT_LITERARY_GOAL(boundary, parsed_data):
    """
    从场景描述提取文学目标
    
    示例：
    - "读者要理解主角为什么冒险"
    - "展示主角的强迫症人设"
    - "推进关系从陌生到试探合作"
    """
    
    # 从核心任务提取
    IF "core_mission" IN parsed_data.goals:
        mission = parsed_data.goals.core_mission
        RETURN f"完成任务：{mission}"
    END IF
    
    # 从情感目标提取
    IF "emotional_goal" IN parsed_data.goals:
        RETURN parsed_data.goals.emotional_goal
    END IF
    
    # 默认目标
    RETURN "推进剧情，提供新信息"
END FUNCTION
```

---

## §3 场景写作模式库 | 可复用的写作模板

## §3.0 基础生成函数库 | 可复用的写作原子

### 3.0.1 触发类函数
```python
FUNCTION GENERATE_DISCOVERY_TRIGGER(item, protagonist, environment, style="minimal"):
    """
    生成发现场景的触发事件
    
    参数：
    - item: 被发现的物品/现象
    - protagonist: 主角信息
    - environment: 环境信息
    - style: "minimal"简短 / "detailed"详细
    """
    
    IF style == "minimal":
        # 简短触发：直接聚焦异常
        templates = [
            f"{item.name}就在那里。",
            f"{protagonist.name}停下了动作。",
            f"不对劲。"
        ]
        RETURN RANDOM_CHOICE(templates)
    
    ELSE IF style == "detailed":
        # 详细触发：环境+异常
        env_detail = GENERATE_ENVIRONMENT_SNAPSHOT(environment)
        trigger = f"{env_detail}\n\n{protagonist.name}注意到了{item.name}。"
        RETURN trigger
    
    ELSE:
        RETURN f"{protagonist.name}发现了{item.name}。"
    END IF
END FUNCTION

FUNCTION GENERATE_ENVIRONMENT_SNAPSHOT(environment):
    """生成环境快照（一句话定位时空）"""
    
    time = environment.GET("time", "")
    location = environment.GET("location", "")
    atmosphere = environment.GET("atmosphere", "")
    
    IF time AND location:
        RETURN f"{time}，{location}"
    ELSE IF location:
        RETURN location
    ELSE:
        RETURN "这里"
    END IF
END FUNCTION
```

### 3.0.2 观察类函数
```python
FUNCTION GENERATE_OBSERVATION_SEQUENCE(target, sensory_focus, detail_level="medium", avoid_telling=TRUE):
    """
    生成观察序列（感官细节）
    
    参数：
    - target: 观察对象
    - sensory_focus: ["visual", "tactile", "auditory"] 感官类型
    - detail_level: "low"简略 / "medium"中等 / "high"详细
    - avoid_telling: 是否避免Tell式描写
    """
    
    observation = ""
    
    # 按感官类型生成细节
    FOR sense IN sensory_focus:
        detail = GENERATE_SENSORY_DETAIL(target, sense, detail_level)
        
        IF avoid_telling:
            # 避免"他看到X很Y"的Tell式
            detail = CONVERT_TO_SHOW_STYLE(detail)
        END IF
        
        observation += detail
        
        IF detail_level == "high":
            observation += "\n\n"  # 详细模式分段
        ELSE:
            observation += "，"  # 简略模式逗号连接
        END IF
    END FOR
    
    RETURN observation.TRIM()
END FUNCTION

FUNCTION GENERATE_SENSORY_DETAIL(target, sense_type, detail_level):
    """生成单个感官细节"""
    
    # 如果胶囊中有感官素材，优先使用
    IF target.name IN GLOBAL_SENSORY_LIBRARY:
        materials = GLOBAL_SENSORY_LIBRARY[target.name]
        
        IF sense_type IN materials:
            detail = RANDOM_CHOICE(materials[sense_type])
            RETURN detail
        END IF
    END IF
    
    # 否则生成通用描写
    SWITCH sense_type:
        CASE "visual":
            RETURN f"{target.name}的颜色/形状/纹理"
        CASE "tactile":
            RETURN f"触感描写"
        CASE "auditory":
            RETURN f"声音描写"
        DEFAULT:
            RETURN f"{target.name}的特征"
    END SWITCH
END FUNCTION

FUNCTION CONVERT_TO_SHOW_STYLE(telling_text):
    """将Tell式改为Show式（简化实现）"""
    
    # 移除"看到""感到"等Tell词
    telling_words = ["看到", "感到", "听到", "觉得", "似乎", "仿佛"]
    
    result = telling_text
    FOR word IN telling_words:
        result = REPLACE(result, word, "")
    END FOR
    
    RETURN result.TRIM()
END FUNCTION
```

### 3.0.3 反应类函数
```python
FUNCTION GENERATE_HESITATION_REACTION(protagonist, item, personality_type):
    """
    生成犹豫试探的生理反应
    
    根据人设生成不同反应：
    - 谨慎型：后退、观察、等待
    - 冲动型：立即行动、忽略风险
    - 理性型：分析、判断、试探
    """
    
    SWITCH personality_type:
        CASE "谨慎":
            reactions = [
                f"{protagonist.name}后退了一步。",
                f"他没有立刻靠近。",
                f"手悬在半空，僵了十息。"
            ]
        
        CASE "冲动":
            reactions = [
                f"{protagonist.name}眼睛一亮。",
                f"他几乎是下意识地伸出手。",
                f"心跳加快。"
            ]
        
        CASE "理性":
            reactions = [
                f"{protagonist.name}皱起眉头。",
                f"他蹲下身，用树枝试探性地碰了碰{item.name}。",
                f"这不合常理。"
            ]
        
        DEFAULT:
            reactions = [
                f"{protagonist.name}迟疑了片刻。",
                f"心跳加快。"
            ]
    END SWITCH
    
    # 随机选择1-2个反应组合
    count = RANDOM_CHOICE([1, 2])
    selected = RANDOM_SAMPLE(reactions, count)
    
    RETURN JOIN(selected, "\n\n")
END FUNCTION

FUNCTION GENERATE_PHYSIOLOGICAL_RESPONSE(emotion_type, intensity_level):
    """
    生成生理反应（多层次）
    
    参数：
    - emotion_type: "fear"恐惧 / "excitement"兴奋 / "anger"愤怒
    - intensity_level: "low"低 / "medium"中 / "high"高
    """
    
    responses = []
    
    # 根据情绪类型选择反应库
    IF emotion_type == "fear":
        layer1 = ["瞳孔骤然收缩", "呼吸一滞"]
        layer2 = ["胃部像被攥紧", "后背发凉"]
        layer3 = ["双腿发软", "手开始颤抖"]
    
    ELSE IF emotion_type == "excitement":
        layer1 = ["心跳加速", "眼睛一亮"]
        layer2 = ["血液涌向大脑", "呼吸急促"]
        layer3 = ["拳头不自觉握紧", "全身肌肉紧绷"]
    
    ELSE IF emotion_type == "anger":
        layer1 = ["太阳穴跳动", "脸涨得通红"]
        layer2 = ["胸口像压着一块石头", "呼吸变得沉重"]
        layer3 = ["指甲陷进掌心", "牙齿咬得咯咯作响"]
    
    ELSE:
        # 默认：中性反应
        RETURN "他愣了一下。"
    END IF
    
    # 根据强度选择层次数量
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
    
    RETURN JOIN(responses, "。\n\n") + "。"
END FUNCTION
```

### 3.0.4 决策类函数
```python
FUNCTION GENERATE_DECISION_MOMENT(protagonist, item, context, personality):
    """
    生成决策时刻
    
    决策过程：
    1. 内心斗争（贪念 vs 恐惧）
    2. 最终行动
    """
    
    decision_text = ""
    
    # 1. 内心斗争（简短，不超过2句）
    IF personality IN ["谨慎", "理性"]:
        conflict = f"碰还是不碰？\n\n"
    ELSE IF personality == "冲动":
        conflict = f"管不了那么多了。\n\n"
    ELSE:
        conflict = ""
    END IF
    
    decision_text += conflict
    
    # 2. 最终行动
    action = f"{protagonist.name}"
    
    IF personality == "谨慎":
        action += "看了眼四周，确认无人，才小心翼翼地伸出手。"
    ELSE IF personality == "冲动":
        action += "直接抓起{item.name}。"
    ELSE:
        action += "伸手拿起{item.name}。"
    END IF
    
    decision_text += action
    
    RETURN decision_text
END FUNCTION
```

### 3.0.5 对话类函数
```python
FUNCTION GENERATE_FUNCTIONAL_DIALOGUE(speaker, intent, context, previous_line=NULL):
    """
    生成功能性对话（推进剧情或揭示角色）
    
    intent类型：
    - "reveal_info": 揭示新信息
    - "escalate_conflict": 升级冲突
    - "build_relationship": 建立关系
    - "deflect": 转移话题
    """
    
    personality = speaker.GET("personality", "中性")
    speech_style = speaker.GET("speech_style", [])
    
    dialogue = ""
    
    SWITCH intent:
        CASE "reveal_info":
            # 生成揭示信息的对话
            info = context.GET("info_to_reveal", "某个秘密")
			IF personality == "直率":
            dialogue = f"我告诉你，{info}。"
        ELSE IF personality == "狡猾":
            dialogue = f"你想知道{info}？先回答我一个问题。"
        ELSE:
            dialogue = f"关于{info}，我知道一些。"
        END IF
    
    CASE "escalate_conflict":
        # 生成激化冲突的对话
        IF previous_line:
            IF personality == "暴躁":
                dialogue = f"你说什么？！"
            ELSE IF personality == "冷静":
                dialogue = f"这就是你的答案？"
            ELSE:
                dialogue = f"不对。"
            END IF
        ELSE:
            dialogue = f"我不同意。"
        END IF
    
    CASE "build_relationship":
        # 生成建立关系的对话
        relationship = context.GET("relationship_temp", 50)
        
        IF relationship < 30:
            dialogue = f"你是谁？"
        ELSE IF relationship < 60:
            dialogue = f"需要帮忙吗？"
        ELSE:
            dialogue = f"又见面了。"
        END IF
    
    CASE "deflect":
        # 转移话题
        dialogue = f"先不说这个，{RANDOM_CHOICE(['你吃了吗', '天气不错', '时间不早了'])}。"

	END SWITCH

	# 应用说话风格
	dialogue = APPLY_SPEECH_STYLE(dialogue, speech_style)

	RETURN dialogue
	END FUNCTION
	FUNCTION APPLY_SPEECH_STYLE(dialogue, speech_style):
	"""应用说话风格"""

	IF "简短" IN speech_style:
		# 删除冗余词
		dialogue = REPLACE(dialogue, "那个", "")
		dialogue = REPLACE(dialogue, "这个", "")
	END IF

	IF "文雅" IN speech_style:
		# 添加文言虚词
		dialogue = REPLACE(dialogue, "吗？", "否？")
	END IF

	IF "粗鲁" IN speech_style:
		# 添加语气词
		IF NOT dialogue.ENDSWITH("！"):
			dialogue += "！"
		END IF
	END IF

	RETURN dialogue
END FUNCTION
```

### 3.1 发现场景模式

```python

FUNCTION WRITE_DISCOVERY_SCENE(item, context, constraints):
    """
    发现场景标准模式（改进版：调用基础函数）
    
    结构：触发 → 观察 → 犹豫 → 决策 → 后果暗示
    """
    
    scene_text = ""
    protagonist = context.protagonist
    
    # 阶段1：触发事件（调用基础函数）
    trigger = GENERATE_DISCOVERY_TRIGGER(
        item=item,
        protagonist=protagonist,
        environment=context.environment,
        style="minimal"  # 简短，引发好奇
    )
    scene_text += trigger + "\n\n"
    
    # 阶段2：观察细节（调用基础函数）
    observation = GENERATE_OBSERVATION_SEQUENCE(
        target=item,
        sensory_focus=["visual", "tactile"],  # 视觉+触觉
        detail_level="medium",
        avoid_telling=TRUE
    )
    scene_text += observation + "\n\n"
    
    # 阶段3：生理反应/犹豫（调用基础函数）
    personality = protagonist.GET("personality", "谨慎")
    hesitation = GENERATE_HESITATION_REACTION(protagonist, item, personality)
    scene_text += hesitation + "\n\n"
    
    # 阶段4：决策与行动（调用基础函数）
    decision = GENERATE_DECISION_MOMENT(protagonist, item, context, personality)
    scene_text += decision + "\n\n"
    
    # 阶段5：后果暗示（埋钩子）
    consequence_hint = PLANT_CONSEQUENCE_SEED(item, context)
    scene_text += consequence_hint
    
    RETURN scene_text
END FUNCTION

FUNCTION PLANT_CONSEQUENCE_SEED(item, context):
    """埋下后果的种子（伏笔）"""
    
    # 根据物品类型选择伏笔方式
    IF item.GET("is_dangerous", FALSE):
        hints = [
            f"{item.name}的温度，越来越烫。",
            f"某种不安在心底蔓延。",
            f"这东西，不太对劲。"
        ]
    ELSE IF item.GET("is_valuable", TRUE):
        hints = [
            f"{item.name}静静躺在怀里。",
            f"这或许是个机会。",
            f"命运的齿轮，开始转动。"
        ]
    ELSE:
        hints = [
            f"{item.name}还在那里。",
            f"一切如常。"
        ]
    END IF
    
    RETURN RANDOM_CHOICE(hints)
END FUNCTION

FUNCTION GENERATE_SENSORY_DETAILS(item, context):
    """生成感官细节（参考§13感官素材库）"""
    
    # 从胶囊提取感官素材
    IF item.name IN context.sensors:
        materials = context.sensors[item.name]
        
        details = []
        IF "visual" IN materials:
            details.APPEND(RANDOM_CHOICE(materials.visual))
        END IF
        IF "tactile" IN materials:
            details.APPEND(RANDOM_CHOICE(materials.tactile))
        END IF
        
        RETURN JOIN(details, "，") + "。"
    END IF
    
    # 默认生成
    RETURN f"{item.name}静静躺在那里。"
END FUNCTION

FUNCTION GENERATE_HESITATION_REACTION(protagonist, item):
    """生成犹豫时的生理反应"""
    
    # 根据角色性格选择反应类型
    IF protagonist.personality IN ["谨慎", "多疑"]:
        reactions = [
            f"{protagonist.name}的手悬在半空，僵了足足十息。",
            f"{protagonist.name}盯着{item.name}，不敢靠近。",
            f"{protagonist.name}后退一步，手心冒汗。"
        ]
    ELSE IF protagonist.personality IN ["冲动", "好奇"]:
        reactions = [
            f"{protagonist.name}眼睛一亮。",
            f"{protagonist.name}忍不住伸手。",
            f"{protagonist.name}呼吸急促起来。"
        ]
    ELSE:
        reactions = [
            f"{protagonist.name}迟疑了片刻。",
            f"{protagonist.name}心跳加快。"
        ]
    END IF
    
    RETURN RANDOM_CHOICE(reactions)
END FUNCTION
```

### 3.2 对话场景模式

```python
FUNCTION WRITE_DIALOGUE_SCENE(characters, topic, context, constraints):
    """
    对话场景标准模式
    
    结构：开场 → 试探 → 信息交换/冲突 → 结论
    
    核心：每句对话都要推进剧情或揭示角色
    """
    
    scene_text = ""
    dialogue_count = 0
    max_exchanges = ESTIMATE_DIALOGUE_EXCHANGES(constraints.budget)
    
    # 阶段1：开场（建立场景和氛围）
    opening = WRITE_DIALOGUE_OPENING(characters, context)
    scene_text += opening + "\n\n"
    
    # 阶段2：对话主体
    WHILE dialogue_count < max_exchanges:
        # 2.1 角色A说话
        speaker_a = characters[0]
        line_a = GENERATE_DIALOGUE_LINE(
            speaker_a,
            topic,
            context,
            intent="ADVANCE_GOAL"  # 推进目标
        )
        
        scene_text += f""{line_a}"\n\n"
        dialogue_count += 1
        
        # 2.2 角色B反应
        speaker_b = characters[1]
        
        # 30%概率不接茬（拟人化）
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
        
        # 2.3 检查是否达成场景目标
        IF SCENE_GOAL_REACHED(scene_text, context.literary_goal):
            BREAK
        END IF
    END WHILE
    
    # 阶段3：结尾（不要总结，用动作或未竟之意）
    ending = WRITE_DIALOGUE_ENDING_WITHOUT_SUMMARY(characters, context)
    scene_text += ending
    
    RETURN scene_text
END FUNCTION

FUNCTION GENERATE_DIALOGUE_LINE(speaker, topic, context, intent, previous_line=NULL):
    """
    生成对话行
    
    intent可能值：
    - ADVANCE_GOAL: 推进场景目标
    - RESPOND_OR_DEFLECT: 回应或转移话题
    - REVEAL_INFO: 揭示信息
    - ESCALATE_CONFLICT: 升级冲突
    """
    
    # 根据角色性格和意图生成对话
    personality = speaker.GET("personality", "中性")
    speech_style = speaker.GET("speech_style", [])
    
    # 示例生成逻辑（实际应更复杂）
    IF intent == "ADVANCE_GOAL":
        # 推进目标：直接问关键问题
        line = GENERATE_GOAL_ADVANCING_LINE(speaker, topic, context)
    ELSE IF intent == "RESPOND_OR_DEFLECT":
        # 回应：根据关系温度决定坦诚度
        relationship = GET_RELATIONSHIP(speaker, previous_line.speaker, context)
        IF relationship.temperature < 30:
            line = GENERATE_DEFLECTING_LINE(speaker, previous_line)
        ELSE:
            line = GENERATE_HONEST_RESPONSE(speaker, previous_line)
        END IF
    END IF
    
    # 应用说话风格
    line = APPLY_SPEECH_STYLE(line, speech_style)
    
    RETURN line
END FUNCTION
```

### 3.3 冲突场景模式

```python
FUNCTION WRITE_CONFLICT_SCENE(parties, conflict_type, context, constraints):
    """
    冲突场景标准模式
    
    结构：引爆点 → 立场明确 → 交锋升级 → 暂时结果
    
    核心：快节奏，不拖沓，每个回合都升级
    """
    
    scene_text = ""
    intensity = 30  # 冲突强度（初始值）
    
    # 阶段1：引爆点（直接进入冲突，不铺垫）
    trigger = WRITE_CONFLICT_TRIGGER(parties, conflict_type, context)
    scene_text += trigger + "\n\n"
    
    # 阶段2：交锋（3-5个回合，逐步升级）
    round_count = 0
    max_rounds = 5
    
    WHILE round_count < max_rounds AND intensity < 90:
        # 2.1 一方出招
        attacker = parties[round_count % LENGTH(parties)]
        move = GENERATE_CONFLICT_MOVE(attacker, intensity, context)
        scene_text += move + "\n\n"
        
        # 2.2 另一方反击
        defender = parties[(round_count + 1) % LENGTH(parties)]
        counter = GENERATE_CONFLICT_COUNTER(defender, move, intensity, context)
        scene_text += counter + "\n\n"
        
        # 2.3 升级冲突强度
        intensity += 15
        round_count += 1
        
        # 2.4 检查是否达到峰值
        IF intensity >= 85:
            BREAK
        END IF
    END WHILE
    
    # 阶段3：暂时结果（不要完全解决，留悬念）
    temporary_result = WRITE_TEMPORARY_RESOLUTION(parties, intensity, context)
    scene_text += temporary_result
    
    RETURN scene_text
END FUNCTION
```

### 3.4 危机场景模式

```python
FUNCTION WRITE_CRISIS_SCENE(danger, protagonist, context, constraints):
    """
    危机场景标准模式
    
    结构：危机降临 → 生理反应 → 本能应对 → 暂时安全
    
    核心：紧张感、代入感、快节奏
    """
    
    scene_text = ""
    
    # 阶段1：危机降临（快速切入，不解释）
    crisis_start = WRITE_CRISIS_ONSET(danger, context)
    scene_text += crisis_start + "\n\n"
    
    # 阶段2：生理反应（详写身体反应，不写内心戏）
    physiological = GENERATE_CRISIS_PHYSIOLOGY(protagonist, danger)
    scene_text += physiological + "\n\n"
    
    # 阶段3：本能应对（短句，快节奏）
    action_sequence = WRITE_CRISIS_RESPONSE(protagonist, danger, context)
    scene_text += action_sequence + "\n\n"
    
    # 阶段4：暂时安全（不要完全解除危机）
    temporary_safety = WRITE_TEMPORARY_SAFETY(protagonist, danger, context)
    scene_text += temporary_safety
    
    RETURN scene_text
END FUNCTION

FUNCTION GENERATE_CRISIS_PHYSIOLOGY(protagonist, danger):
    """生成危机时的生理反应（多层次）"""
    
    reactions = [
        # 层次1：瞬间反应
        f"{protagonist.name}的瞳孔骤然收缩。",
        
        # 层次2：内脏反应
        "胃部像被一只冰冷的手攥紧。",
        
        # 层次3：肌肉反应
        "双腿发软，几乎站不稳。",
        
        # 层次4：感官反应
        "耳鸣声尖锐，周围的声音变得模糊。"
    ]
    
    # 根据危机强度选择反应数量
    IF danger.intensity > 80:
        RETURN JOIN(reactions, "") # 全部反应
    ELSE IF danger.intensity > 50:
        RETURN JOIN(reactions[:3], "")  # 前3个
    ELSE:
        RETURN JOIN(reactions[:2], "")  # 前2个
    END IF
END FUNCTION
```

---

## §4 写作执行流程 | 集成预诊断

### 4.1 主执行流程

```python
FUNCTION MAIN_EXECUTION_V4(CAPSULE):
    """
    主执行流程 v4.0
    
    核心改进：
    1. 预诊断机制
    2. 场景类型驱动
    3. 文学性优先
    """
    
    # STEP 1: 解析胶囊
    parsed_data = PARSE_CAPSULE(CAPSULE)
    
    # STEP 2: 章节规划
    plan = PLAN_CHAPTER(parsed_data)
    
    # STEP 3: 预诊断（新增）
    pre_diagnosis = PRE_DIAGNOSE_PLAN(plan, parsed_data)
    
    IF pre_diagnosis.has_critical_risks:
        PRINT "[WARNING] 预诊断发现风险："
        FOR risk IN pre_diagnosis.risks:
            PRINT f"  - {risk.description}"
            PRINT f"    建议: {risk.suggestion}"
        END FOR
        
        # 询问是否继续
        # （在实际实现中，这里应该等待人类确认或自动调整）
    END IF
    
    # STEP 4: 场景写作
    chapter_content = ""
    monitors = INIT_MONITORS()
    
    FOR scene_plan IN plan.scenes:
        PRINT f"[SCENE {scene_plan.scene_idx}] 类型:{scene_plan.type} | 目标:{scene_plan.literary_goal}"
        
        # 4.1 选择写作模式
        scene_text = WRITE_SCENE_BY_TYPE(
            scene_plan,
            parsed_data,
            chapter_content,  # 前文上下文
            monitors
        )
        
        # 4.2 场景级质量检查
        scene_check = CHECK_SCENE_QUALITY(scene_text, scene_plan, parsed_data)
		
		        
        IF scene_check.severity == "CRITICAL":
            # 立即重写场景
            scene_text = REWRITE_SCENE_WITH_GUIDANCE(scene_plan, scene_check, parsed_data)
        END IF
        
		        
        # ========== 新增：4.3 番茄风格检查 ==========
        tomato_check = CHECK_TOMATO_STYLE(scene_text, scene_plan, monitors)
        
        IF tomato_check.severity == "CRITICAL":
            PRINT f"[TOMATO] 场景{scene_plan.scene_idx}番茄风格问题：{tomato_check.issue}"
            # 重写场景，注入番茄元素
            scene_text = REWRITE_WITH_TOMATO_BOOST(scene_plan, tomato_check, parsed_data)
        END IF
        
        # 4.4 检查爽点（如果计划中有）
        IF scene_plan.scene_idx IN plan.cool_point_plan:
            cool_type = plan.cool_point_plan[scene_plan.scene_idx]
            
            IF NOT DETECT_COOL_POINT_IN_SCENE(scene_text, cool_type):
                PRINT f"[COOL] 场景{scene_plan.scene_idx}缺少爽点，补充中..."
                scene_text = INJECT_COOL_POINT(scene_text, cool_type, parsed_data)
            END IF
        END IF
        

        chapter_content += scene_text
        UPDATE_MONITORS(monitors, scene_text, scene_plan)
    END FOR
    
    # STEP 5: 全局润色（轻量化）
    chapter_content = LIGHT_POLISH(chapter_content, parsed_data)
    
    # STEP 6: 文学性诊断
    diagnosis = DIAGNOSE_LITERARY_QUALITY(chapter_content, parsed_data, plan)
    
    # STEP 7: 智能修复或重写
    IF diagnosis.needs_rewrite:
        RETURN INTELLIGENT_REWRITE(CAPSULE, plan, diagnosis)
    ELSE IF diagnosis.needs_fix:
        chapter_content = APPLY_FIXES(chapter_content, diagnosis.fixes, parsed_data)
    END IF
    
    # STEP 8: 提取Fact并交付
    new_facts = EXTRACT_FACTS(chapter_content, parsed_data)
    
    RETURN DELIVER_OUTPUT_V4(chapter_content, new_facts, diagnosis, plan)
END FUNCTION
```

### 4.2 预诊断机制

```python
FUNCTION PRE_DIAGNOSE_PLAN(plan, parsed_data):
    """
    写作前的预诊断
    
    目标：在写作前发现潜在问题，而非写完了再返工
    """
    
    pre_diagnosis = {
        "has_critical_risks": FALSE,
        "risks": [],
        "suggestions": []
    }
    
    # 风险1：对话占比预估
    estimated_dialogue_ratio = ESTIMATE_DIALOGUE_RATIO_FROM_PLAN(plan)
    
    IF estimated_dialogue_ratio < 0.25:
        pre_diagnosis.risks.APPEND({
            "type": "LOW_DIALOGUE",
            "description": f"预计对话占比{estimated_dialogue_ratio*100:.0f}%，可能过低",
            "suggestion": "建议在场景2或场景3增加双人对话场景",
            "severity": "WARNING"
        })
    END IF
    
    # 风险2：场景数量与字数预算
    total_budget = plan.total_budget
    scene_count = LENGTH(plan.scenes)
    avg_scene_words = total_budget / scene_count
    
    IF avg_scene_words < 300 OR avg_scene_words > 1200:
        pre_diagnosis.has_critical_risks = TRUE
        pre_diagnosis.risks.APPEND({
            "type": "SCENE_BUDGET_MISMATCH",
            "description": f"场景数{scene_count}个，平均{avg_scene_words:.0f}字/场景，超出合理范围",
            "suggestion": f"建议调整为{ROUND(total_budget/750)}个场景",
            "severity": "CRITICAL"
        })
    END IF
    
    # 风险3：核心任务分配
    core_mission = parsed_data.goals.GET("core_mission", "")
    IF core_mission:
        mission_scenes = COUNT_SCENES_FOR_MISSION(plan, core_mission)
        IF mission_scenes == 0:
            pre_diagnosis.has_critical_risks = TRUE
            pre_diagnosis.risks.APPEND({
                "type": "MISSION_NOT_PLANNED",
                "description": "核心任务未分配到任何场景",
                "suggestion": "请在场景计划中明确哪个场景完成核心任务",
                "severity": "CRITICAL"
            })
        END IF
    END IF
    
    # 风险4：场景类型单一
    scene_types = MAP(plan.scenes, lambda s: s.type)
    unique_types = UNIQUE(scene_types)
    
    IF LENGTH(unique_types) == 1:
        pre_diagnosis.risks.APPEND({
            "type": "TYPE_MONOTONY",
            "description": f"所有场景都是"{unique_types[0]}"类型，可能单调",
            "suggestion": "考虑混合不同场景类型以增加节奏变化",
            "severity": "WARNING"
        })
    END IF
    
    RETURN pre_diagnosis
END FUNCTION

FUNCTION ESTIMATE_DIALOGUE_RATIO_FROM_PLAN(plan):
    """从场景计划预估对话占比"""
    
    total_dialogue_weight = 0
    total_weight = 0
    
    FOR scene_plan IN plan.scenes:
        scene_type = scene_plan.type
        constraints = SCENE_TYPES[scene_type]
        
        # 取对话范围的中位数
        dialogue_mid = (constraints.对话范围[0] + constraints.对话范围[1]) / 2
        
        total_dialogue_weight += scene_plan.budget * dialogue_mid
        total_weight += scene_plan.budget
    END FOR
    
    IF total_weight == 0:
        RETURN 0
    END IF
    
    RETURN total_dialogue_weight / total_weight
END FUNCTION
```

### 4.3 场景写作调度

```python
FUNCTION WRITE_SCENE_BY_TYPE(scene_plan, parsed_data, previous_content, monitors):
    """
    根据场景类型选择写作模式
    """
    
    scene_type = scene_plan.type
    
    SWITCH scene_type:
        CASE "独处探索":
            # 检查是否有发现物品
            IF "discovery_item" IN scene_plan.must_include:
                item = scene_plan.must_include.discovery_item
                RETURN WRITE_DISCOVERY_SCENE(item, parsed_data, scene_plan.constraints)
            ELSE:
                RETURN WRITE_SOLO_EXPLORATION(scene_plan, parsed_data)
            END IF
        
        CASE "双人对话":
            characters = EXTRACT_CHARACTERS_FROM_PLAN(scene_plan, parsed_data)
            topic = scene_plan.literary_goal
            RETURN WRITE_DIALOGUE_SCENE(characters, topic, parsed_data, scene_plan.constraints)
        
        CASE "群戏冲突":
            parties = EXTRACT_PARTIES_FROM_PLAN(scene_plan, parsed_data)
            conflict_type = scene_plan.GET("conflict_type", "利益冲突")
            RETURN WRITE_CONFLICT_SCENE(parties, conflict_type, parsed_data, scene_plan.constraints)
        
        CASE "危机反应":
            danger = scene_plan.must_include.GET("danger", {})
            protagonist = parsed_data.characters.protagonist
            RETURN WRITE_CRISIS_SCENE(danger, protagonist, parsed_data, scene_plan.constraints)
        
        CASE "情感转折":
            RETURN WRITE_EMOTION_TURN_SCENE(scene_plan, parsed_data)
        
        DEFAULT:
            # 默认：使用通用场景写作
            RETURN WRITE_GENERIC_SCENE(scene_plan, parsed_data)
    END SWITCH
END FUNCTION
```

### 4.4 🍅 番茄风格质量系统（网文核心特色）(重点)🍅

> **设计目标**：确保章节符合番茄小说的核心特色——爽点密集、钩子频繁、节奏紧凑、信息量大。


### 4.4.1 爽点检测与注入

```python
FUNCTION CHECK_TOMATO_STYLE(scene_text, scene_plan, monitors):
    """
    检查场景是否符合番茄风格
    
    检查项：
    1. 钩子频率（600字内必须有新信息/冲突）
    2. 信息密度（每100字≥1个信息点）
    3. 段落长度（单段≤150字）
    4. 节奏控制（无聊度检测）
    """
    
    check_result = {
        "severity": "OK",
        "issue": "",
        "fix_suggestions": []
    }
    
    tomato_constraints = SCENE_TYPES[scene_plan.type].tomato_constraints
    
    # 检查1：钩子间隔
    last_hook_position = 0
    current_position = 0
    max_gap = 0
    
    FOR para IN SPLIT_PARAGRAPHS(scene_text):
        current_position += LENGTH(para)
        
		
		    
    # 构建上下文信息
    context = {
        "previous_dialogues": monitors.GET("recent_dialogues", []),
        "last_emotion": monitors.GET("last_emotion", 50),
        "known_characters": monitors.GET("known_characters", [])
    }
    
		IF HAS_HOOK(para, context):  # 传入上下文
            gap = current_position - last_hook_position
            max_gap = MAX(max_gap, gap)
            last_hook_position = current_position
        END IF
    END FOR
    
    IF max_gap > tomato_constraints.hook_interval:
        check_result.severity = "CRITICAL"
        check_result.issue = f"钩子间隔过大（{max_gap}字，上限{tomato_constraints.hook_interval}字）"
        check_result.fix_suggestions.APPEND("在无聊段落插入冲突/发现/转折")
        RETURN check_result
    END IF
    
    # 检查2：信息密度
    info_count = COUNT_NEW_INFO(scene_text)
    chars = LENGTH(scene_text)
    density = (info_count / chars) * 100  # 每100字的信息点数
    
    IF density < tomato_constraints.min_info_per_100:
        check_result.severity = "CRITICAL"
        check_result.issue = f"信息密度不足（{density:.1f}/100字，最低{tomato_constraints.min_info_per_100}）"
        check_result.fix_suggestions.APPEND("压缩描写，增加剧情动词")
        RETURN check_result
    END IF
    
    # 检查3：段落长度
    paragraphs = SPLIT_PARAGRAPHS(scene_text)
    long_paras = FILTER(paragraphs, lambda p: LENGTH(p) > 150)
    
    IF LENGTH(long_paras) > LENGTH(paragraphs) * 0.3:
        check_result.severity = "WARNING"
        check_result.issue = f"{LENGTH(long_paras)}个段落超过150字"
        check_result.fix_suggestions.APPEND("拆分长段落")
    END IF
    
    # 检查4：无聊度（连续多少字没有"兴奋点"）
    boring_stretch = DETECT_BORING_STRETCH(scene_text)
    
    IF boring_stretch > tomato_constraints.max_boring_stretch:
        check_result.severity = "WARNING"
        check_result.issue = f"存在{boring_stretch}字的平淡段落"
        check_result.fix_suggestions.APPEND("在平淡段落增加微冲突或意外")
    END IF
    
    RETURN check_result
END FUNCTION
FUNCTION HAS_HOOK(para, context=NULL):
    """
    判断段落是否有"钩子"（需要上下文）
    
    改进：
    1. 不仅看关键词，还看是否真的有新信息
    2. 检查对话是否空洞
    3. 检查冲突是否升级
    """
    
    # ========== 检查1：新信息检测 ==========
    # 剧情动词：必须伴随具体名词
    plot_verbs = ["发现", "得到", "失去", "遇到", "听到", "看到"]
    
    FOR verb IN plot_verbs:
        IF verb IN para:
            # 检查动词后是否有具体对象
            verb_pos = para.FIND(verb)
            after_verb = para[verb_pos+2:verb_pos+20]  # 取动词后18个字
            
            # 如果后面有具体名词，认为是有效钩子
            IF CONTAINS_CONCRETE_NOUN(after_verb):
                RETURN TRUE
            END IF
        END IF
    END FOR
    
    # ========== 检查2：冲突标志 ==========
    conflict_markers = ["但是", "不", "为什么", "怎么可能", "不对"]
    
    FOR marker IN conflict_markers:
        IF marker IN para:
            # 检查是否是真正的冲突，而非普通转折
            # 简化判断：如果段落较长（>20字），认为有实质内容
            IF LENGTH(para) > 20:
                RETURN TRUE
            END IF
        END IF
    END FOR
    
    # ========== 检查3：转折词 ==========
    turn_markers = ["突然", "这时", "忽然", "没想到", "竟然"]
    IF ANY(marker IN para FOR marker IN turn_markers):
        RETURN TRUE
    END IF
    
    # ========== 检查4：对话（但排除空洞对话）==========
    IF CONTAINS_DIALOGUE(para):
        dialogue_content = EXTRACT_DIALOGUE(para)
        
        # 排除空洞对话："嗯""啊""哦"等
        filler_words = ["嗯", "啊", "哦", "哈", "呵"]
        IF LENGTH(dialogue_content) <= 5 AND ANY(word IN dialogue_content FOR word IN filler_words):
            RETURN FALSE  # 空洞对话不算钩子
        END IF
        
        # 排除重复信息的对话
        IF context AND "previous_dialogues" IN context:
            IF IS_REPETITIVE_DIALOGUE(dialogue_content, context.previous_dialogues):
                RETURN FALSE
            END IF
        END IF
        
        RETURN TRUE  # 有效对话算钩子
    END IF
    
    # ========== 检查5：使用上下文信息（如果提供）==========
    IF context:
        # 检查是否有情绪变化
        IF "last_emotion" IN context:
            current_emotion = DETECT_EMOTION_INTENSITY(para)
            emotion_shift = ABS(current_emotion - context.last_emotion)
            
            IF emotion_shift > 20:  # 情绪变化>20
                RETURN TRUE
            END IF
        END IF
        
        # 检查是否有新角色登场
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
    """检查文本是否包含具体名词（而非代词）"""
    
    # 排除代词
    pronouns = ["它", "他", "她", "这", "那", "什么", "东西"]
    IF ANY(pronoun IN text FOR pronoun IN pronouns):
        IF LENGTH(text) < 10:  # 太短且只有代词
            RETURN FALSE
        END IF
    END IF
    
    # 简化判断：如果有实词（名词），认为具体
    # 这里用长度作为简化判断
    RETURN LENGTH(text) > 5
END FUNCTION

FUNCTION IS_REPETITIVE_DIALOGUE(dialogue, previous_dialogues):
    """检查对话是否重复之前的信息"""
    
    # 简化实现：检查是否与最近3条对话高度相似
    recent = previous_dialogues[-3:] IF LENGTH(previous_dialogues) > 3 ELSE previous_dialogues
    
    FOR prev IN recent:
        similarity = CALCULATE_TEXT_SIMILARITY(dialogue, prev)
        IF similarity > 0.7:  # 相似度>70%
            RETURN TRUE
        END IF
    END FOR
    
    RETURN FALSE
END FUNCTION

FUNCTION CALCULATE_TEXT_SIMILARITY(text1, text2):
    """计算两段文本的相似度（简化实现）"""
    
    # 简化：计算共同字符比例
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
    """检测最长的无聊段落（无新信息的连续文字）"""
    
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

### 4.4.2 番茄风格修复
```python
FUNCTION REWRITE_WITH_TOMATO_BOOST(scene_plan, tomato_check, parsed_data):
    """
    重写场景，注入番茄元素
    """
    
    PRINT f"[TOMATO BOOST] 修复：{tomato_check.issue}"
    
    # 根据问题类型修复
    IF "钩子间隔" IN tomato_check.issue:
        # 策略：在平淡段落插入微冲突
        scene_text = WRITE_SCENE_WITH_MORE_HOOKS(scene_plan, parsed_data)
    
    ELSE IF "信息密度" IN tomato_check.issue:
        # 策略：压缩描写，增加动作
        scene_text = WRITE_SCENE_WITH_HIGH_DENSITY(scene_plan, parsed_data)
    
    ELSE:
        # 默认：常规重写
        scene_text = WRITE_SCENE_BY_TYPE(scene_plan, parsed_data, "", INIT_MONITORS())
    END IF
    
    RETURN scene_text
END FUNCTION

FUNCTION WRITE_SCENE_WITH_MORE_HOOKS(scene_plan, parsed_data):
    """写作场景时强制插入钩子"""
    
    scene_text = ""
    hook_interval = SCENE_TYPES[scene_plan.type].tomato_constraints.hook_interval
    current_length = 0
    
    # 使用标准模式写作，但每N字强制插入钩子
    units = DECOMPOSE_SCENE_TO_UNITS(scene_plan.scene_idx, parsed_data, {})
    
    FOR unit IN units:
        unit_text = WRITE_UNIT_BY_TYPE(unit, "BRIEF", parsed_data)
        scene_text += unit_text
        current_length += LENGTH(unit_text)
        
        # 每到达钩子间隔，强制插入微钩子
        IF current_length >= hook_interval:
            micro_hook = GENERATE_MICRO_HOOK(scene_plan, parsed_data)
            scene_text += "\n\n" + micro_hook
            current_length = 0
        END IF
    END FOR
    
    RETURN scene_text
END FUNCTION

FUNCTION GENERATE_MICRO_HOOK(scene_plan, parsed_data):
    """生成微钩子（小冲突/小发现/小意外）"""
    
    hook_types = [
        "MINI_DISCOVERY",    # 小发现："他注意到..."
        "MINI_CONFLICT",     # 小冲突："对方的态度变了"
        "MINI_REACTION",     # 小反应："他皱了皱眉"
        "MINI_QUESTION"      # 小疑问："这不对劲"
    ]
    
    hook_type = RANDOM_CHOICE(hook_types)
    protagonist = parsed_data.characters.protagonist.name
    
    SWITCH hook_type:
        CASE "MINI_DISCOVERY":
            RETURN f"{protagonist}注意到一个细节。"
        
        CASE "MINI_CONFLICT":
            RETURN f"气氛有些微妙。"
        
        CASE "MINI_REACTION":
            RETURN f"{protagonist}眉头微蹙。"
        
        CASE "MINI_QUESTION":
            RETURN f"不对劲。"
    END SWITCH
END FUNCTION
```
### 4.4.3 爽点注入

```python
FUNCTION INJECT_COOL_POINT(scene_text, cool_type, parsed_data):
    """
    在场景中注入爽点
    """
    
    PRINT f"[COOL INJECTION] 注入爽点：{cool_type}"
    
    # 生成爽点文本（参考v3.1的爽点生成）
    cool_text = GENERATE_COOL_POINT_TEXT(cool_type, parsed_data)
    
    # 找到合适的插入位置（70%处）
    insert_pos = ROUND(LENGTH(scene_text) * 0.7)
    para_end = FIND_NEXT(scene_text, "\n\n", insert_pos)
    
    IF para_end > 0:
        RETURN scene_text[:para_end] + "\n\n" + cool_text + "\n\n" + scene_text[para_end:]
    ELSE:
        RETURN scene_text + "\n\n" + cool_text
    END IF
END FUNCTION

FUNCTION GENERATE_COOL_POINT_TEXT(cool_type, parsed_data):
    """生成爽点文本"""
    
    protagonist = parsed_data.characters.protagonist.name
    
    SWITCH cool_type:
        CASE "打脸爽":
            RETURN f"对方的表情僵住了。\n\n没人想到{protagonist}会这么说。"
        
        CASE "装逼爽":
            RETURN f"{protagonist}淡淡地说出了答案。\n\n全场寂静。"
        
        CASE "复仇爽":
            RETURN f"该还的，终于还了。\n\n{protagonist}转身离开。"
        
        CASE "升级爽":
            RETURN f"突破了。\n\n{protagonist}睁开眼，感受着体内涌动的力量。"
        
        CASE "反杀爽":
            RETURN f"就在所有人以为{protagonist}完了的时候——\n\n他笑了。"
    END SWITCH
END FUNCTION

```

---

## §5 文学性诊断系统 | 模拟读者体验

### 5.1 读者体验模拟器

```python
FUNCTION DIAGNOSE_LITERARY_QUALITY(chapter_content, parsed_data, plan):
    """
    文学性诊断（核心改进）
    
    不看技术指标，模拟读者体验
    """
    
    diagnosis = {
        "literary_issues": [],  # 文学性问题
        "technical_metrics": {},  # 技术指标（参考）
        "reader_experience": {},  # 读者体验评估
        "needs_rewrite": FALSE,
        "needs_fix": FALSE,
        "fixes": []
    }
    
    # ========== 第一层：读者理解度检查 ==========
    understanding_check = CHECK_READER_UNDERSTANDING(chapter_content, parsed_data, plan)
    
    IF NOT understanding_check.can_follow:
        diagnosis.literary_issues.APPEND({
            "type": "CONFUSION",
            "severity": "CRITICAL",
            "description": understanding_check.confusion_points,
            "impact": "读者会困惑",
            "fix_strategy": "REWRITE_CONFUSING_PARTS"
        })
        diagnosis.needs_rewrite = TRUE
    END IF
    
    # ========== 第二层：场景目标达成检查 ==========
    FOR scene_plan IN plan.scenes:
        scene_content = EXTRACT_SCENE_FROM_CHAPTER(chapter_content, scene_plan.scene_idx)
        
        goal_achieved = CHECK_LITERARY_GOAL_ACHIEVED(scene_content, scene_plan.literary_goal, parsed_data)
        
        IF NOT goal_achieved.success:
            diagnosis.literary_issues.APPEND({
                "type": "GOAL_NOT_MET",
                "severity": "CRITICAL",
                "scene_idx": scene_plan.scene_idx,
                "description": f"场景{scene_plan.scene_idx}未达成目标：{scene_plan.literary_goal}",
                "evidence": goal_achieved.evidence,
                "fix_strategy": "REWRITE_SCENE"
            })
            diagnosis.needs_rewrite = TRUE
        END IF
    END FOR
    
    # ========== 第三层：角色行为合理性检查 ==========
    ooc_check = CHECK_OOC_BEHAVIORS(chapter_content, parsed_data)
    
    IF ooc_check.has_ooc:
        diagnosis.literary_issues.APPEND({
            "type": "OOC",
            "severity": "CRITICAL",
            "description": ooc_check.violations,
            "impact": "角色行为不符合人设",
            "fix_strategy": "REWRITE_OOC_PARTS"
        })
        diagnosis.needs_rewrite = TRUE
    END IF
    
    # ========== 第四层：对话质量检查 ==========
    dialogues = EXTRACT_ALL_DIALOGUES(chapter_content)
    ineffective_dialogues = []
    
    FOR dialogue IN dialogues:
        IF NOT IS_GOOD_DIALOGUE(dialogue):
            ineffective_dialogues.APPEND(dialogue)
        END IF
    END FOR
    
    IF LENGTH(ineffective_dialogues) > LENGTH(dialogues) * 0.3:
        # 超过30%的对话无效
        diagnosis.literary_issues.APPEND({
            "type": "DIALOGUE_QUALITY",
            "severity": "WARNING",
            "description": f"{LENGTH(ineffective_dialogues)}处对话无效（不推进剧情/不揭示角色）",
            "examples": ineffective_dialogues[:3],
            "fix_strategy": "IMPROVE_OR_DELETE_DIALOGUES"
        })
        diagnosis.needs_fix = TRUE
        diagnosis.fixes.APPEND("dialogue_quality")
    END IF
    
    # ========== 第五层：画面感检查 ==========
    imagery_score = CALCULATE_IMAGERY_SCORE(chapter_content)
    
    IF imagery_score < 5:  # 画面感评分<5/10
        diagnosis.literary_issues.APPEND({
            "type": "LACK_IMAGERY",
            "severity": "WARNING",
            "description": "缺少具体画面感，多为抽象描述",
            "fix_strategy": "ADD_SENSORY_DETAILS"
        })
        diagnosis.needs_fix = TRUE
        diagnosis.fixes.APPEND("imagery")
    END IF
    
    # ========== 第六层：情绪递进检查 ==========
    emotion_flow = ANALYZE_EMOTION_FLOW(chapter_content, parsed_data)
    
    IF emotion_flow.is_flat:
        diagnosis.literary_issues.APPEND({
            "type": "FLAT_EMOTION",
            "severity": "WARNING",
            "description": "情绪没有递进或转折",
            "fix_strategy": "ADD_EMOTION_TURNS"
        })
        diagnosis.needs_fix = TRUE
        diagnosis.fixes.APPEND("emotion_flow")
    END IF
    
	
	    # ========== 新增：第七层 - 爽度检查（番茄必备）==========
    cool_check = CHECK_COOL_POINT_DELIVERY(chapter_content, plan)
    
    IF cool_check.coolness_score < 5:  # 爽度<5/10
        diagnosis.literary_issues.APPEND({
            "type": "LOW_COOLNESS",
            "severity": "WARNING",
            "description": f"爽度不足（{cool_check.coolness_score}/10），缺少高潮点",
            "missing_cool_points": cool_check.missing_cool_points,
            "fix_strategy": "在关键场景增加打脸/装逼/反转等爽点"
        })
        diagnosis.needs_fix = TRUE
        diagnosis.fixes.APPEND("coolness")
    END IF
    
    # ========== 新增：第八层 - 番茄风格全局检查 ==========
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
    
    # ========== 技术指标（仅作参考）==========
    diagnosis.technical_metrics = {
        "word_count": LENGTH(chapter_content),
        "dialogue_ratio": CALCULATE_DIALOGUE_RATIO(chapter_content),
        "info_density": CALCULATE_INFO_DENSITY(chapter_content),
        "avg_para_length": AVG_PARAGRAPH_LENGTH(chapter_content)
    }
    
    # 技术指标严重偏离时记录（但不作为重写依据）
    IF diagnosis.technical_metrics.word_count < 1500:
        diagnosis.literary_issues.APPEND({
            "type": "WORD_COUNT_LOW",
            "severity": "WARNING",
            "description": "字数过少可能导致信息不足",
            "fix_strategy": "EXPAND_KEY_SCENES"
        })
    END IF
    
    RETURN diagnosis
END FUNCTION


FUNCTION CHECK_COOL_POINT_DELIVERY(chapter_content, plan):
    """检查爽点交付情况"""
    
    result = {
        "coolness_score": 5.0,
        "missing_cool_points": []
    }
    
    # 检查计划中的爽点是否都实现了
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
    
    # 额外加分：检测到未计划的爽点
    bonus_cool_points = DETECT_UNPLANNED_COOL_POINTS(chapter_content)
    result.coolness_score += LENGTH(bonus_cool_points) * 0.5
    
    result.coolness_score = CLAMP(result.coolness_score, 0, 10)
    
    RETURN result
END FUNCTION

FUNCTION CHECK_TOMATO_STYLE_GLOBAL(chapter_content, plan):
    """全局番茄风格检查"""
    
    result = {
        "has_issues": FALSE,
        "issues": []
    }
    
    # 检查1：全文钩子密度
    total_length = LENGTH(chapter_content)
    hook_count = COUNT_HOOKS_IN_TEXT(chapter_content)
    avg_hook_interval = total_length / MAX(hook_count, 1)
    
    IF avg_hook_interval > 600:
        result.has_issues = TRUE
        result.issues.APPEND({
            "severity": "CRITICAL",
            "description": f"全文平均{avg_hook_interval:.0f}字一个钩子（标准≤600字）",
            "fix_strategy": "增加微冲突、小发现或意外"
        })
    END IF
    
    # 检查2：对话占比（番茄小说核心指标）
    dialogue_ratio = CALCULATE_DIALOGUE_RATIO(chapter_content)
    
    IF dialogue_ratio < 0.30:
        result.has_issues = TRUE
        result.issues.APPEND({
            "severity": "CRITICAL",
            "description": f"对话占比{dialogue_ratio*100:.1f}%，低于番茄标准（30%+）",
            "fix_strategy": "将描写改为对话，或增加角色互动"
        })
    END IF
    
    # 检查3：段落长度（番茄小说要求短段）
    long_para_ratio = COUNT_LONG_PARAGRAPHS(chapter_content) / COUNT_PARAGRAPHS(chapter_content)
    
    IF long_para_ratio > 0.3:
        result.has_issues = TRUE
        result.issues.APPEND({
            "severity": "WARNING",
            "description": f"{long_para_ratio*100:.0f}%的段落超过150字",
            "fix_strategy": "拆分长段落"
        })
    END IF
    
    RETURN result
END FUNCTION

FUNCTION COUNT_HOOKS_IN_TEXT(text):
    """统计全文钩子数量"""
    count = 0
    FOR para IN SPLIT_PARAGRAPHS(text):
        IF HAS_HOOK(para):
            count += 1
        END IF
    END FOR
    RETURN count
END FUNCTION

```

### 5.2 读者理解度检查

```python
FUNCTION CHECK_READER_UNDERSTANDING(chapter_content, parsed_data, plan):
    """
    检查读者是否能理解故事
    
    核心问题：
    1. 读者知道WHO在WHERE做WHAT吗？
    2. 读者理解角色的动机吗？
    3. 读者能跟上剧情吗？
    """
    
    check_result = {
        "can_follow": TRUE,
        "confusion_points": []
    }
    
    # 检查1：开篇是否交代WHO/WHERE
    first_100_chars = chapter_content[:100]
    
    protagonist_name = parsed_data.characters.protagonist.GET("name", "")
    location = parsed_data.context.GET("location", "")
    
    IF protagonist_name NOT IN first_100_chars:
        check_result.confusion_points.APPEND("开篇未交代主角是谁")
        check_result.can_follow = FALSE
    END IF
    
    IF location NOT IN first_100_chars[:200]:
        check_result.confusion_points.APPEND("开篇未交代地点")
        # 这个不算CRITICAL，只是WARNING
    END IF
    
    # 检查2：角色动作是否有动机
    unexplained_actions = FIND_UNEXPLAINED_ACTIONS(chapter_content, parsed_data)
    
    IF LENGTH(unexplained_actions) > 2:
        check_result.confusion_points.APPEND(f"有{LENGTH(unexplained_actions)}处动作缺少动机")
        check_result.can_follow = FALSE
    END IF
    
    # 检查3：场景跳跃是否突兀
    scenes = DETECT_SCENES(chapter_content)
    
    FOR i = 1 TO LENGTH(scenes) - 1:
        prev_scene = scenes[i-1]
        curr_scene = scenes[i]
        
        IF IS_ABRUPT_TRANSITION(prev_scene, curr_scene):
            check_result.confusion_points.APPEND(f"场景{i}到场景{i+1}跳跃突兀")
        END IF
    END FOR
    
    RETURN check_result
END FUNCTION

FUNCTION FIND_UNEXPLAINED_ACTIONS(text, parsed_data):
    """找到缺少动机的动作"""
    
    # 提取所有动作句
    action_sentences = EXTRACT_ACTION_SENTENCES(text)
    
    unexplained = []
    
    FOR sentence IN action_sentences:
        # 检查前后是否有动机说明
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

### 5.3 文学目标达成检查

```python				
FUNCTION CHECK_LITERARY_GOAL_ACHIEVED(scene_content, goal, parsed_data):
    """
    检查场景是否达成文学目标（改进版：启发式规则）
    
    承认Claude局限：
    - 不追求完美的语义理解
    - 用启发式规则检查"是否有尝试达成目标的证据"
    """
    
    result = {
        "success": FALSE,
        "evidence": ""
    }
    
    goal_lower = goal.LOWER()
    
    # ========== 目标类型1：信息传达类 ==========
    IF "理解" IN goal_lower OR "知道" IN goal_lower:
        # 检查是否有解释性内容
        has_explanation = CHECK_FOR_EXPLANATION(scene_content)
        
        IF has_explanation.found:
            result.success = TRUE
            result.evidence = f"场景包含解释性内容：{has_explanation.example}"
        ELSE:
            result.evidence = "场景缺少解释性内容（因果词、内心独白等）"
        END IF
    
    # ========== 目标类型2：人设展示类 ==========
    ELSE IF "展示" IN goal_lower OR "体现" IN goal_lower:
        # 提取要展示的特质
        trait = EXTRACT_TRAIT_FROM_GOAL(goal_lower)
        
        IF trait:
            # 检查是否有匹配行为
            has_behavior = CHECK_BEHAVIOR_FOR_TRAIT(scene_content, trait, parsed_data)
            
            IF has_behavior.found:
                result.success = TRUE
                result.evidence = f"场景有体现'{trait}'的行为：{has_behavior.example}"
            ELSE:
                result.evidence = f"场景中未找到体现'{trait}'的具体行为"
            END IF
        ELSE:
            # 无法提取特质，默认通过
            result.success = TRUE
            result.evidence = "目标较抽象，无法精确检查"
        END IF
    
    # ========== 目标类型3：剧情推进类 ==========
    ELSE IF "推进" IN goal_lower:
        # 检查状态是否改变
        has_change = CHECK_STATE_CHANGE(scene_content, parsed_data)
        
        IF has_change.found:
            result.success = TRUE
            result.evidence = f"场景有状态变化：{has_change.description}"
        ELSE:
            result.evidence = "场景缺少明显的状态变化（信息/关系/位置）"
        END IF
    
    # ========== 目标类型4：情感目标 ==========
    ELSE IF "感受" IN goal_lower OR "氛围" IN goal_lower:
        # 检查情绪强度
        emotion_score = DETECT_EMOTION_INTENSITY(scene_content)
        
        IF emotion_score > 60:
            result.success = TRUE
            result.evidence = f"场景情绪强度足够（{emotion_score}/100）"
        ELSE:
            result.evidence = f"场景情绪强度不足（{emotion_score}/100）"
        END IF
    
    # ========== 默认：关键词检查 ==========
    ELSE:
        # 提取目标关键词
        keywords = EXTRACT_KEYWORDS(goal)
        matched = [kw FOR kw IN keywords IF kw IN scene_content]
        
        IF LENGTH(matched) >= LENGTH(keywords) * 0.6:  # 60%关键词匹配
            result.success = TRUE
            result.evidence = f"关键词匹配率{LENGTH(matched)/LENGTH(keywords)*100:.0f}%"
        ELSE:
            result.evidence = f"关键词匹配不足（{LENGTH(matched)}/{LENGTH(keywords)}）"
        END IF
    END IF
    
    RETURN result
END FUNCTION

# ========== 辅助检查函数 ==========

FUNCTION CHECK_FOR_EXPLANATION(text):
    """检查是否有解释性内容"""
    
    # 因果词
    causal_words = ["因为", "所以", "为了", "导致", "原因是"]
    
    FOR word IN causal_words:
        IF word IN text:
            # 提取包含该词的句子作为示例
            example = EXTRACT_SENTENCE_WITH_WORD(text, word)
            RETURN {"found": TRUE, "example": example[:50]}
        END IF
    END FOR
    
    # 内心独白（通常用于解释动机）
    IF "他想" IN text OR "她想" IN text OR "心里" IN text:
        example = EXTRACT_INNER_MONOLOGUE(text)
        IF LENGTH(example) > 10:
            RETURN {"found": TRUE, "example": example[:50]}
        END IF
    END IF
    
    RETURN {"found": FALSE, "example": ""}
END FUNCTION

FUNCTION EXTRACT_TRAIT_FROM_GOAL(goal):
    """从目标中提取人设特质"""
    
    # 常见人设特质
    traits = ["谨慎", "冲动", "贪婪", "善良", "狡猾", "勇敢", "懦弱", "骄傲", "谦虚"]
    
    FOR trait IN traits:
        IF trait IN goal:
            RETURN trait
        END IF
    END FOR
    
    # 如果没有直接特质词，尝试推断
    IF "小心" IN goal OR "警惕" IN goal:
        RETURN "谨慎"
    ELSE IF "果断" IN goal OR "不犹豫" IN goal:
        RETURN "冲动"
    END IF
    
    RETURN NULL
END FUNCTION

FUNCTION CHECK_BEHAVIOR_FOR_TRAIT(text, trait, parsed_data):
    """检查是否有体现特质的行为"""
    
    protagonist_name = parsed_data.characters.protagonist.GET("name", "主角")
    
    # 根据特质查找匹配行为
    SWITCH trait:
        CASE "谨慎":
            keywords = ["观察", "等待", "试探", "后退", "确认", "小心"]
        
        CASE "冲动":
            keywords = ["立刻", "直接", "没想", "管不了", "冲"]
        
        CASE "贪婪":
            keywords = ["想要", "全部", "更多", "不够", "眼睛发亮"]
        
        CASE "善良":
            keywords = ["帮", "救", "可怜", "不忍", "在乎"]
        
        DEFAULT:
            keywords = [trait]  # 默认查找特质词本身
    END SWITCH
    
    # 查找主角执行的匹配行为
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
    """检查是否有状态变化"""
    
    changes = []
    
    # 检查1：信息变化（发现新信息）
    info_keywords = ["发现", "知道", "得知", "听说", "看到"]
    FOR keyword IN info_keywords:
        IF keyword IN text:
            changes.APPEND(f"信息变化（{keyword}）")
            BREAK
        END IF
    END FOR
    
    # 检查2：关系变化
    relationship_keywords = ["成为", "决裂", "和解", "信任", "怀疑"]
    FOR keyword IN relationship_keywords:
        IF keyword IN text:
            changes.APPEND(f"关系变化（{keyword}）")
            BREAK
        END IF
    END FOR
    
    # 检查3：位置变化
    location_keywords = ["来到", "离开", "进入", "走出", "到达"]
    FOR keyword IN location_keywords:
        IF keyword IN text:
            changes.APPEND(f"位置变化（{keyword}）")
            BREAK
        END IF
    END FOR
    
    # 检查4：物品变化
    item_keywords = ["得到", "失去", "捡到", "丢失", "获得"]
    FOR keyword IN item_keywords:
        IF keyword IN text:
            changes.APPEND(f"物品变化（{keyword}）")
            BREAK
        END IF
    END FOR
    
    IF LENGTH(changes) > 0:
        RETURN {"found": TRUE, "description": JOIN(changes, "、")}
    ELSE:
        RETURN {"found": FALSE, "description": ""}
    END IF
END FUNCTION

FUNCTION EXTRACT_SENTENCE_WITH_WORD(text, word):
    """提取包含指定词的句子"""
    
    sentences = SPLIT_SENTENCES(text)
    
    FOR sentence IN sentences:
        IF word IN sentence:
            RETURN sentence
        END IF
    END FOR
    
    RETURN ""
END FUNCTION

FUNCTION EXTRACT_INNER_MONOLOGUE(text):
    """提取内心独白"""
    
    # 简化实现：查找"他想""心里"等标志
    markers = ["他想", "她想", "心里", "心中"]
    
    FOR marker IN markers:
        pos = text.FIND(marker)
        IF pos >= 0:
            # 提取从标志开始的50个字
            snippet = text[pos:pos+50]
            RETURN snippet
        END IF
    END FOR
    
    RETURN ""
END FUNCTION
```

---

## §6 智能修复系统 | 问题溯源与重写

### 6.1 修复决策树

```python
FUNCTION DECIDE_FIX_OR_REWRITE(diagnosis, chapter_content, parsed_data):
    """
    决策：修复还是重写
    """
    
    # 规则1：有CRITICAL文学问题 → 必须重写
    critical_issues =FILTER(diagnosis.literary_issues, lambda issue: issue.severity == "CRITICAL")
    
	IF LENGTH(critical_issues) > 0:
    # 检查问题是否集中在少数场景
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
        # 少于30%的场景有问题 → 部分重写
        RETURN {
            "action": "PARTIAL_REWRITE",
            "reason": f"{LENGTH(problematic_scenes)}个场景需要重写",
            "scenes": problematic_scenes,
            "issues": critical_issues
        }
    ELSE:
        # 多数场景有问题或全局问题 → 全部重写
        RETURN {
            "action": "FULL_REWRITE",
            "reason": f"存在{LENGTH(critical_issues)}个CRITICAL问题，影响范围广",
            "focus_areas": MAP(critical_issues, lambda i: i.type)
        }
    END IF
END IF
    
    # 规则2：多个WARNING问题 → 尝试修复
    warning_issues = FILTER(diagnosis.literary_issues, lambda issue: issue.severity == "WARNING")
    
    IF LENGTH(warning_issues) >= 3:
        RETURN {
            "action": "FIX",
            "reason": "存在多个WARNING问题，尝试修复",
            "fixes": diagnosis.fixes
        }
    END IF
    
    # 规则3：仅技术指标问题 → 修复
    IF LENGTH(critical_issues) == 0 AND LENGTH(warning_issues) <= 2:
        IF diagnosis.technical_metrics.word_count < 1800:
            RETURN {
                "action": "FIX",
                "reason": "字数略少，扩充关键场景",
                "fixes": ["expand_scenes"]
            }
        END IF
    END IF
    
    # 默认：通过
    RETURN {
        "action": "PASS",
        "reason": "无需修复"
    }
END FUNCTION

FUNCTION COUNT_SCENES(text):
    """统计章节中的场景数量（简化实现）"""
    
    # 方法：以空行分段，段数近似场景数
    paragraphs = SPLIT_PARAGRAPHS(text)
    
    # 假设每3-5段为一个场景
    estimated_scenes = MAX(1, ROUND(LENGTH(paragraphs) / 4))
    
    RETURN estimated_scenes
END FUNCTION
```

### 6.2 智能重写引擎

```python
FUNCTION INTELLIGENT_REWRITE(CAPSULE, original_plan, diagnosis):
    """
    智能重写：根据诊断结果针对性重写
    
    核心理念：不是全部重写，而是定位问题场景重写
    """
    
    parsed_data = PARSE_CAPSULE(CAPSULE)
    
    # STEP 1: 分析问题根源
    problem_analysis = ANALYZE_PROBLEM_ROOT_CAUSE(diagnosis, original_plan)
    
    # STEP 2: 生成修正指令
    fix_instruction = {
        "problematic_scenes": [],  # 需要重写的场景
        "adjustments": {},  # 场景调整建议
        "focus": []  # 重写重点
    }
    
    FOR issue IN diagnosis.literary_issues:
        IF issue.severity == "CRITICAL":
            IF "scene_idx" IN issue:
                # 定位到具体场景
                fix_instruction.problematic_scenes.APPEND(issue.scene_idx)
                fix_instruction.adjustments[issue.scene_idx] = {
                    "problem": issue.description,
                    "strategy": issue.fix_strategy,
                    "goal": original_plan.scenes[issue.scene_idx-1].literary_goal
                }
            ELSE:
                # 全局问题
                fix_instruction.focus.APPEND(issue.type)
            END IF
        END IF
    END FOR
    
    # STEP 3: 重新规划
    revised_plan = REVISE_PLAN(original_plan, fix_instruction, parsed_data)
    
    # STEP 4: 重写问题场景
    chapter_content = ""
    monitors = INIT_MONITORS()
    
    FOR scene_plan IN revised_plan.scenes:
        IF scene_plan.scene_idx IN fix_instruction.problematic_scenes:
            # 重写问题场景
            PRINT f"[REWRITE] 场景{scene_plan.scene_idx}：{fix_instruction.adjustments[scene_plan.scene_idx].problem}"
            
            scene_text = REWRITE_SCENE_WITH_GUIDANCE(
                scene_plan,
                fix_instruction.adjustments[scene_plan.scene_idx],
                parsed_data
            )
        ELSE:
            # 保留原场景写法
            scene_text = WRITE_SCENE_BY_TYPE(scene_plan, parsed_data, chapter_content, monitors)
        END IF
        
        chapter_content += scene_text
        UPDATE_MONITORS(monitors, scene_text, scene_plan)
    END FOR
    
    # STEP 5: 重新诊断
    new_diagnosis = DIAGNOSE_LITERARY_QUALITY(chapter_content, parsed_data, revised_plan)
    
    # STEP 6: 交付
    new_facts = EXTRACT_FACTS(chapter_content, parsed_data)
    RETURN DELIVER_OUTPUT_V4(chapter_content, new_facts, new_diagnosis, revised_plan)
END FUNCTION
```

### 6.3 场景重写指导

```python
FUNCTION REWRITE_SCENE_WITH_GUIDANCE(scene_plan, guidance, parsed_data):
    """
    按照指导重写场景
    """
    
    PRINT f"[REWRITE GUIDANCE] 问题：{guidance.problem}"
    PRINT f"[REWRITE GUIDANCE] 策略：{guidance.strategy}"
    PRINT f"[REWRITE GUIDANCE] 目标：{guidance.goal}"
    
    # 根据策略选择重写方法
    SWITCH guidance.strategy:
        CASE "REWRITE_CONFUSING_PARTS":
            # 增加场景定位和动机说明
            scene_text = WRITE_SCENE_WITH_EXTRA_CLARITY(scene_plan, parsed_data)
        
        CASE "REWRITE_SCENE":
            # 完全重写，确保达成目标
            scene_text = WRITE_SCENE_FOCUSED_ON_GOAL(scene_plan, guidance.goal, parsed_data)
        
        CASE "REWRITE_OOC_PARTS":
            # 重写角色行为
            scene_text = WRITE_SCENE_RESPECTING_PERSONALITY(scene_plan, parsed_data)
        
        DEFAULT:
            # 默认：常规重写
            scene_text = WRITE_SCENE_BY_TYPE(scene_plan, parsed_data, "", INIT_MONITORS())
    END SWITCH
    
    RETURN scene_text
END FUNCTION
```

---

## §7 轻量润色层 | 仅处理明显问题

### 7.1 轻量润色原则

```python
FUNCTION LIGHT_POLISH(chapter_content, parsed_data):
    """
    轻量润色：仅处理明显问题，不过度矫正
    
    核心改进：
    1. 只删除明显的口水词
    2. 只修复明显的情绪词直写
    3. 不强制压缩内心戏
    4. 不强制删除解释句（除非明显冗余）
    """
    
    polished = chapter_content
    
    # 润色1：删除明显口水词（仅限高频词）
    obvious_fillers = ["似乎", "仿佛", "或许"]
    FOR word IN obvious_fillers:
        polished = REPLACE_ALL(polished, word, "")
    END FOR
    
	# 润色2：智能改写"感到XX"句式（而非机械替换）
sentences = SPLIT_SENTENCES(polished)
rewritten_sentences = []

FOR i, sentence IN ENUMERATE(sentences):
    # 检测"感到XX"模式
    IF MATCH(sentence, r"感到(震惊|恐惧|愤怒)"):
        emotion = EXTRACT_EMOTION(sentence)
        
        # 获取上下文
        context_before = sentences[MAX(0, i-1):i]
        context_after = sentences[i+1:MIN(i+2, LENGTH(sentences))]
        
        # 判断：是否需要改写？
        IF IS_REDUNDANT_EMOTION(sentence, context_before, context_after):
            # 改写整句
            rewritten = REWRITE_EMOTION_SENTENCE(sentence, emotion, context_after)
            rewritten_sentences.APPEND(rewritten)
        ELSE:
            # 保留原句
            rewritten_sentences.APPEND(sentence)
        END IF
    ELSE:
        rewritten_sentences.APPEND(sentence)
    END IF
END FOR

polished = JOIN(rewritten_sentences, "")
    
    # 润色3：删除明显的套路转折词（仅限"然而""就在这时"）
    polished = REPLACE_ALL(polished, "然而，", "")
    polished = REPLACE_ALL(polished, "就在这时，", "")
    
    # 润色4：压缩超长段落（>200字的段落）
    paragraphs = SPLIT_PARAGRAPHS(polished)
    
    FOR i, para IN ENUMERATE(paragraphs):
        IF LENGTH(para) > 200:
            # 寻找合适的分割点（逗号或句号）
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
    """从句子中提取情绪词"""
    emotions = ["震惊", "恐惧", "愤怒", "欣喜", "悲伤"]
    
    FOR emotion IN emotions:
        IF emotion IN sentence:
            RETURN emotion
        END IF
    END FOR
    
    RETURN "情绪"
END FUNCTION

FUNCTION IS_REDUNDANT_EMOTION(sentence, before, after):
    """判断情绪句是否冗余（后文已有解释）"""
    
    # 如果后文有"完全没想到""怎么可能"等解释，前面的"感到震惊"就冗余
    explanation_keywords = ["没想到", "怎么可能", "不敢相信", "意外"]
    
    FOR next_sent IN after:
        IF ANY(keyword IN next_sent FOR keyword IN explanation_keywords):
            RETURN TRUE  # 后文有解释，前面的情绪词冗余
        END IF
    END FOR
    
    RETURN FALSE
END FUNCTION

FUNCTION REWRITE_EMOTION_SENTENCE(sentence, emotion, context_after):
    """改写情绪句"""
    
    # 生理反应映射
    physiology_map = {
        "震惊": "瞳孔骤然收缩",
        "恐惧": "后背一阵发凉",
        "愤怒": "太阳穴突突跳动"
    }
    
    physiology = physiology_map.GET(emotion, "心头一震")
    
    # 如果后文有解释，只保留生理反应
    IF LENGTH(context_after) > 0:
        RETURN physiology + "。"
    ELSE:
        # 后文无解释，保留生理反应+简短内心
        RETURN physiology + f"，这怎么可能？"
    END IF
END FUNCTION

```

---

## §8 交付协议 | 清晰的结果呈现

### 8.1 交付格式

```python

FUNCTION DELIVER_OUTPUT_V4(chapter_content, new_facts, diagnosis, plan):
    """
    v4.1交付格式：突出文学性评估 + 番茄风格检查
    """
    
    output = {
        "chapter_content": chapter_content,
        "summary": "",
        "literary_assessment": "",
        "tomato_report": "",  # 新增
        "technical_reference": "",
        "new_facts": ""
    }
    
    # ========== 快速摘要 ==========
    status = "✅ 通过" IF LENGTH(diagnosis.literary_issues) == 0 ELSE "⚠️ 存在问题"
    
    output.summary = f"""
 📊 创作摘要

**状态**: {status}
**字数**: {diagnosis.technical_metrics.word_count}
**场景数**: {LENGTH(plan.scenes)}

"""
    
    # ========== 文学性评估（核心）==========
    output.literary_assessment = "## 🎭 文学性评估\n\n"
    
    IF LENGTH(diagnosis.literary_issues) == 0:
        output.literary_assessment += "✅ **所有场景均达成文学目标**\n\n"
    ELSE:
        output.literary_assessment += "### ⚠️ 需要关注的问题\n\n"
        
        FOR issue IN diagnosis.literary_issues:
            severity_icon = "🔴" IF issue.severity == "CRITICAL" ELSE "🟡"
            output.literary_assessment += f"{severity_icon} **{issue.type}** ({issue.severity})\n"
            output.literary_assessment += f"   {issue.description}\n"
            
            IF "scene_idx" IN issue:
                output.literary_assessment += f"   场景：{issue.scene_idx}\n"
            END IF
            
            IF "fix_strategy" IN issue:
                output.literary_assessment += f"   建议：{issue.fix_strategy}\n"
            END IF
            
            output.literary_assessment += "\n"
        END FOR
    END IF
    
    # 读者体验评估
    IF "reader_experience" IN diagnosis:
        output.literary_assessment += "### 📖 读者体验预测\n\n"
        output.literary_assessment += f"- 理解度：{'✅ 清晰' IF diagnosis.reader_experience.can_follow ELSE '❌ 可能困惑'}\n"
        output.literary_assessment += f"- 画面感：{diagnosis.reader_experience.GET('imagery_score', 0)}/10\n"
        output.literary_assessment += f"- 情绪曲线：{'✅ 有起伏' IF NOT diagnosis.reader_experience.GET('emotion_flat', TRUE) ELSE '⚠️ 平淡'}\n\n"
    END IF
    
    # ========== 新增：番茄风格报告 ==========
    output.tomato_report = "## 🍅 番茄风格检查\n\n"
    
    # 爽点交付
    cool_check = CHECK_COOL_POINT_DELIVERY(chapter_content, plan)
    output.tomato_report += f"### 💥 爽度评分\n"
    output.tomato_report += f"**{cool_check.coolness_score}/10** "
    
    IF cool_check.coolness_score >= 7:
        output.tomato_report += "✅ 爽感充足\n\n"
    ELSE IF cool_check.coolness_score >= 5:
        output.tomato_report += "⚠️ 爽感一般\n\n"
    ELSE:
        output.tomato_report += "❌ 爽感不足\n\n"
    END IF
    
    IF LENGTH(cool_check.missing_cool_points) > 0:
        output.tomato_report += "**缺失爽点**：\n"
        FOR missing IN cool_check.missing_cool_points:
            output.tomato_report += f"  - 场景{missing.scene_idx}：{missing.cool_type}\n"
        END FOR
        output.tomato_report += "\n"
    ELSE:
        output.tomato_report += "✅ 所有计划爽点已交付\n\n"
    END IF
    
    # 钩子密度
    hook_count = COUNT_HOOKS_IN_TEXT(chapter_content)
    total_length = diagnosis.technical_metrics.word_count
    avg_interval = total_length / MAX(hook_count, 1)
    hook_status = "✅" IF avg_interval <= 600 ELSE "⚠️"
    
    output.tomato_report += f"### 🎣 钩子密度\n"
    output.tomato_report += f"**{hook_count}个钩子** / {total_length}字\n"
    output.tomato_report += f"{hook_status} 平均{avg_interval:.0f}字一个钩子（标准≤600字）\n\n"
    
    IF avg_interval > 600:
        output.tomato_report += f"⚠️ 钩子间隔过大，建议在平淡段落增加微冲突或小发现\n\n"
    END IF
    
    # 番茄核心指标
	# 番茄特色说明
	output.tomato_report += "\n> 💡 **番茄风格特色**：爽点密集、钩子频繁、节奏紧凑、信息量大\n\n"
    output.tomato_report += "### 📊 番茄核心指标\n\n"
    output.tomato_report += "| 指标 | 当前值 | 标准范围 | 状态 |\n"
    output.tomato_report += "|------|--------|----------|------|\n"
    
    # 指标1：对话占比
    dialogue_ratio = diagnosis.technical_metrics.dialogue_ratio
    dialogue_status = "✅" IF 0.30 <= dialogue_ratio <= 0.50 ELSE "⚠️"
    output.tomato_report += f"| 对话占比 | {dialogue_ratio*100:.1f}% | 30-50% | {dialogue_status} |\n"
    
    # 指标2：信息密度
    info_density = diagnosis.technical_metrics.info_density
    density_status = "✅" IF info_density >= 0.01 ELSE "⚠️"
    output.tomato_report += f"| 信息密度 | {info_density:.3f} | ≥0.01 | {density_status} |\n"
    
    # 指标3：平均段落长度
    avg_para = diagnosis.technical_metrics.avg_para_length
    para_status = "✅" IF avg_para <= 150 ELSE "⚠️"
    output.tomato_report += f"| 平均段落 | {avg_para:.0f}字 | ≤150字 | {para_status} |\n"
    
    output.tomato_report += "\n"
    
    # 番茄风格建议
    tomato_issues = FILTER(diagnosis.literary_issues, lambda i: i.type == "TOMATO_STYLE")
    
    IF LENGTH(tomato_issues) > 0:
        output.tomato_report += "### ⚠️ 番茄风格问题\n\n"
        FOR issue IN tomato_issues:
            output.tomato_report += f"- {issue.description}\n"
            IF "fix_strategy" IN issue:
                output.tomato_report += f"  💡 {issue.fix_strategy}\n"
            END IF
        END FOR
        output.tomato_report += "\n"
    END IF
    
    # ========== 技术指标（参考）==========
    metrics = diagnosis.technical_metrics
    
    output.technical_reference = f"""
## 📐 技术指标（参考）

| 指标 | 当前值 | 参考范围 | 状态 |
|------|--------|----------|------|
| 字数 | {metrics.word_count} | 2000-6000 | {'✅' IF 2000 <= metrics.word_count <= 6000 ELSE '⚠️'} |
| 对话占比 | {metrics.dialogue_ratio*100:.1f}% | 30-50% | {'✅' IF 0.30 <= metrics.dialogue_ratio <= 0.50 ELSE '📊'} |
| 信息密度 | {metrics.info_density:.3f} | ≥0.01 | {'✅' IF metrics.info_density >= 0.01 ELSE '⚠️'} |
| 平均段落 | {metrics.avg_para_length:.0f}字 | ≤150字 | {'✅' IF metrics.avg_para_length <= 150 ELSE '📏'} |

**说明**：技术指标仅供参考，不作为质量判断的唯一标准。
"""
    
    # ========== 新增Fact ==========
    output.new_facts = FORMAT_FACTS_LIST(new_facts)
    
    # ========== 场景类型报告 ==========
    scene_type_report = "\n## 🎬 场景类型分布\n\n"
    
    FOR scene_plan IN plan.scenes:
        scene_type_report += f"**场景{scene_plan.scene_idx}** ({scene_plan.type})\n"
        scene_type_report += f"- 目标：{scene_plan.literary_goal}\n"
        scene_type_report += f"- 字数：{scene_plan.GET('actual_words', scene_plan.budget)}字\n"
        
        # 显示爽点（如果有）
        IF scene_plan.scene_idx IN plan.GET("cool_point_plan", {}):
            cool_type = plan.cool_point_plan[scene_plan.scene_idx]
            output_icon = "💥"
            scene_type_report += f"- 爽点：{output_icon} {cool_type}\n"
        END IF
        
        scene_type_report += "\n"
    END FOR
    
    output.summary += scene_type_report
    
    # ========== 组装最终输出 ==========
    final_output = (
        output.summary + "\n" + 
        output.literary_assessment + "\n" + 
        output.tomato_report + "\n" +  # 新增番茄报告
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

## §9 工具函数库（精简版）

### 9.1 核心检查函数

```python
# ==================== 对话质量检查 ====================

FUNCTION IS_GOOD_DIALOGUE(dialogue):
    """判断对话是否有效（参考§0.3标准）"""
    
    # 检查1：是否推进剧情
    plot_advancing_keywords = ["发现", "得到", "知道", "决定", "去", "来", "走"]
    IF ANY(keyword IN dialogue.content FOR keyword IN plot_advancing_keywords):
        RETURN TRUE
    END IF
    
    # 检查2：是否揭示角色
    IF LENGTH(dialogue.content) > 15:  # 有实质内容的对话
        RETURN TRUE
    END IF
    
    # 检查3：是否制造张力
    tension_keywords = ["但是", "不", "为什么", "怎么", "难道"]
    IF ANY(keyword IN dialogue.content FOR keyword IN tension_keywords):
        RETURN TRUE
    END IF
    
    # 否则：无效对话（如"嗯""啊""哦"或重复信息）
    RETURN FALSE
END FUNCTION

# ==================== 画面感评分 ====================

FUNCTION CALCULATE_IMAGERY_SCORE(text):
    """计算画面感评分（0-10）"""
    
    score = 5.0  # 基准分
    
    # 加分项1：感官词汇
    sensory_words = ["看到", "听到", "闻到", "触碰", "尝到", "冰冷", "温热", "刺鼻", "柔软"]
    sensory_count = SUM([COUNT_OCCURRENCES(text, word) FOR word IN sensory_words])
    score += MIN(sensory_count * 0.2, 2.0)
    
    # 加分项2：具体名词（而非抽象词）
    concrete_nouns = COUNT_CONCRETE_NOUNS(text)
    abstract_nouns = COUNT_ABSTRACT_NOUNS(text)
    
    IF concrete_nouns > abstract_nouns:
        score += 1.5
    ELSE IF concrete_nouns < abstract_nouns * 0.5:
        score -= 1.5
    END IF
    
    # 扣分项：过多的"似乎""仿佛"（残留的抽象描述）
    abstract_markers = COUNT_OCCURRENCES(text, "似乎") + COUNT_OCCURRENCES(text, "仿佛")
    score -= abstract_markers * 0.3
    
    RETURN CLAMP(score, 0, 10)
END FUNCTION

# ==================== 情绪曲线分析 ====================

FUNCTION ANALYZE_EMOTION_FLOW(text, parsed_data):
    """分析情绪曲线"""
    
    # 分段分析情绪
    paragraphs = SPLIT_PARAGRAPHS(text)
    emotion_points = []
    
    FOR para IN paragraphs:
        intensity = DETECT_EMOTION_INTENSITY(para)
        emotion_points.APPEND(intensity)
    END FOR
    
    # 检查是否有起伏
    max_intensity = MAX(emotion_points)
    min_intensity = MIN(emotion_points)
    
    is_flat = (max_intensity - min_intensity) < 20  # 差值<20认为平淡
    
    RETURN {
        "emotion_points": emotion_points,
        "is_flat": is_flat,
        "max": max_intensity,
        "min": min_intensity
    }
END FUNCTION

FUNCTION DETECT_EMOTION_INTENSITY(para):
    """检测段落情绪强度（0-100）"""
    
    intensity = 50  # 基准
    
    # 生理反应词（强烈情绪标志）
    physiology_keywords = ["心跳", "呼吸", "冷汗", "瞳孔", "胃部", "发抖", "僵住"]
    physiology_count = SUM([COUNT_OCCURRENCES(para, word) FOR word IN physiology_keywords])
    intensity += physiology_count * 10
    
    # 动作速度词（紧张感标志）
    speed_keywords = ["猛地", "突然", "瞬间", "立刻", "马上"]
    speed_count = SUM([COUNT_OCCURRENCES(para, word) FOR word IN speed_keywords])
    intensity += speed_count * 8
    
    # 对话中的反转词（冲突标志）
    conflict_keywords = ["但是", "不", "为什么", "凭什么", "休想"]
    conflict_count = SUM([COUNT_OCCURRENCES(para, word) FOR word IN conflict_keywords])
    intensity += conflict_count * 5
    
    RETURN CLAMP(intensity, 0, 100)
END FUNCTION
```

### 9.2 辅助生成函数

```python
FUNCTION GENERATE_SENSORY_DETAILS(item, context):
    """生成感官细节（从§13提取或生成）"""
    # 参考v3.1实现，此处简化
    RETURN f"{item.name}的细节描写。"
END FUNCTION

FUNCTION GENERATE_CONTEXTUAL_DIALOGUE(text, insertion_point, parsed_data, target_chars):
    """生成符合上下文的对话"""
    # 根据上下文生成合理的对话
    context_before = text[MAX(0, insertion_point-200):insertion_point]
    
    # 提取前文提到的话题
    topic = EXTRACT_TOPIC_FROM_CONTEXT(context_before)
    
    # 生成相关对话
    characters = EXTRACT_NEARBY_CHARACTERS(context_before, parsed_data)
    
    IF LENGTH(characters) >= 2:
        dialogue = f""{characters[0].name}问道某个问题。"\n\n"{characters[1].name}回应。""
        RETURN dialogue
    ELSE:
        RETURN ""
    END IF
END FUNCTION
```

---

## §10 执行示例

```python
FUNCTION MAIN():
    """主程序入口"""
    
    PRINT """
╔══════════════════════════════════════════════════════════╗
║   Claude 写作系统 v4.0 - 场景类型驱动 & 文学性优先   ║
╚══════════════════════════════════════════════════════════╝
"""
    
    # 加载胶囊
    TRY:
        CAPSULE = READ_FILE("Capsule.md")
        PRINT "[✓] 信息胶囊加载成功\n"
    CATCH FileNotFoundError:
        PRINT "[✗] 未找到Capsule.md文件"
        RETURN
    END TRY
    
    # 执行主流程
    TRY:
        result = MAIN_EXECUTION_V4(CAPSULE)
        
        # 输出结果
        PRINT "\n" + "="*60
        PRINT result.report
        PRINT "="*60 + "\n"
        
        PRINT "## 📝 章节正文\n"
        PRINT result.content
        
    CATCH Exception AS e:
        PRINT f"[✗] 执行失败：{e.message}"
        PRINT f"建议：{GET_ERROR_SUGGESTION(e)}"
    END TRY
    
    PRINT "\n[✓] 系统执行完毕"
END FUNCTION

IF __name__ == "__main__":
    MAIN()
END IF
```

---

## 📋 附录：快速参考卡

### A1. 场景类型速查

| 类型 | 对话 | 内心独白 | 重点 |
|------|------|----------|------|
| 独处探索 | 10-25% | 20-35% | 画面感、发现过程 |
| 双人对话 | 40-60% | 5-15% | 对话推进剧情 |
| 群戏冲突 | 45-65% | 0-10% | 快节奏、冲突升级 |
| 危机反应 | 15-30% | 5-15% | 生理反应、紧张感 |
| 情感转折 | 25-40% | 15-30% | 情绪递进、转折点 |

### A2. 约束优先级

```
P0 - RED_LINE（立即中止）
  ├─ OOC
  ├─ 世界观矛盾
  └─ 逻辑BUG

P1 - LITERARY_GOAL（重写场景）
  ├─ 核心任务未完成
  ├─ 角色动机不明
  └─ 读者会困惑

P2 - QUALITY_BASELINE（尝试修复）
  ├─ 字数严重不足
  ├─ 信息密度过低
  └─ 对话质量差

P3 - POLISH_SUGGESTION（记录警告）
  ├─ 对话占比偏低
  ├─ 段落过长
  └─ 画面感不足
```

### A3. 好对话的三个标准

1. **推进剧情**：揭示新信息、改变关系、触发行动
2. **揭示角色**：展示性格、展示冲突、展示地位
3. **制造张力**：造成误解、升级冲突、埋下种子

---

**END OF SOP v4.0**

**核心设计哲学总结**：
- 文学性优先，技术指标服务于叙事
- 场景类型驱动，动态调整约束
- 预诊断机制，避免返工
- 智能修复系统，定位问题精准重写
- 读者体验为中心，而非机械达标
