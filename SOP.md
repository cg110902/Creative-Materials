# 📋 Claude写作执行SOP v3.0


---

## 【模块0】GLOBAL_CONFIG - 全局配置

```python
# ==================== 全局常量 ====================
CONST WORD_COUNT_TARGET = 4000  # 目标字数
CONST WORD_COUNT_MIN = 3000     # 最小字数（硬约束）
CONST WORD_COUNT_MAX = 6000     # 最大字数（硬约束）
CONST MAX_REWRITE_ATTEMPTS = 2  # 最大重写次数

# ==================== 番茄风格核心规则 ====================
CONST TOMATO_CORE_RULES = {
    "pace": "快节奏，不拖泥带水",
    "hook_frequency": 600,           # 每600字一个小钩子
    "dialogue_ratio": [0.35, 0.45],  # 对话占比35%-45%
    "inner_monologue_max": 0.15,     # 内心戏不超过15%
    "paragraph_max_length": 150,     # 单段不超过150字
    "info_density_min": 0.01,        # 每100字至少1个信息点
    "conflict_intensity_floor": 30,  # 冲突强度底线
    "boredom_threshold": 60          # 无聊度阈值
}

# ==================== Claude病症完整清单（20条）====================
CONST CLAUDE_ALL20_DISEASES = {
    "D1_过度展开": "每个信息点都展开200字",
    "D2_过度解释": "每个逻辑都要给读者解释清楚",
    "D3_过度内心戏": "主角一个小动作配500字心理活动",
    "D4_过度安全": "遣词造句谨慎得像在写法律文书",  # 【新增】
    "D5_过度冗余": "同一个意思用3种方式重复表达",      # 【新增】
    "D6_过度平均": "每个场景、每个角色的笔墨分配都很均匀", # 【新增】
    "D7_过度逻辑": "所有行为都要有明确的因果链",        # 【新增】
    "D8_过度完整": "每个场景都要有'起承转合'",          # 【新增】
    "D9_过度礼貌": "角色说话都很客气，没有粗鲁/暴躁/省略", # 【新增】
    "D10_过度对称": "主角做A，配角就会回应B，节奏呆板",  # 【新增】
    "D11_信息密度不足": "用200字讲一个只需要20字的事",   # 【新增】
    "D12_口水词滥用": "大量的'似乎''仿佛''或许''可能'等模糊词",
    "D13_套路化转折": "总是用'然而''但是''就在这时'等老套转折词",
    "D14_过度铺垫": "一个小事件前面铺垫300字",          # 【新增】
    "D15_结尾总结癖": "每个场景结尾都要来一句总结性的话",
    "D16_不敢留白": "所有伏笔都要暗示，生怕读者看不懂",
    "D17_不敢跳跃": "时间/空间切换都要详细交代过渡",     # 【新增】
    "D18_情绪词滥用": "动不动就'震惊''恐惧''愤怒'等强情绪词",
    "D19_动作链完整癖": "'他站起来，走到门口，伸手推开门，迈步走出去'",
    "D20_视角漂移": "明明是主角视角，却突然知道了配角的想法"
}

# ==================== 常用爽点类型（精简到5种）====================
CONST COOL_POINT_TYPES = {
    "打脸爽": {
        "结构": "被质疑 → 瞬间反转 → 对方震惊",
        "强度": 80,
        "使用场景": ["展示实力", "拆穿伪装", "反驳质疑"]
    },
    "装逼爽": {
        "结构": "低调隐藏实力 → 关键时刻展现 → 众人震撼",
        "强度": 85,
        "使用场景": ["危机时刻", "被轻视后", "保护他人"]
    },
    "复仇爽": {
        "结构": "被欺负 → 记下仇恨 → 当场或延后报复",
        "强度": 90,
        "使用场景": ["被羞辱", "被陷害", "被背叛"]
    },
    "升级爽": {
        "结构": "突破境界 → 实力暴涨 → 立刻验证",
        "强度": 95,
        "使用场景": ["修炼", "顿悟", "获得传承"]
    },
    "反杀爽": {
        "结构": "濒死绝境 → 突然翻盘 → 反杀敌人",
        "强度": 100,
        "使用场景": ["生死危机", "绝境", "背水一战"]
    }
}

# ==================== 具体性等级表（简化）====================
CONST SPECIFICITY_RULES = {
    "importance >= 8": "具体",    # "他伸出右手，五指微曲，握住门把手"
    "importance 5-7": "中等",     # "他握住门把手"
    "importance < 5": "抽象"      # "他开门"
}

# ==================== 节奏控制规则（用句式代替波形）====================
CONST RHYTHM_RULES = {
    "快节奏": {
        "sentence_length": [5, 15],      # 句长5-15字
        "paragraph_length": [30, 80],    # 段长30-80字
        "pattern": "短句+短句+短句"
    },
    "中节奏": {
        "sentence_length": [15, 25],
        "paragraph_length": [80, 120],
        "pattern": "长短句交错"
    },
    "慢节奏": {
        "sentence_length": [20, 40],
        "paragraph_length": [100, 150],
        "pattern": "长句，详细描写"
    }
}

# ==================== Show vs Tell规则（动态）====================
CONST SHOW_TELL_RULES = {
    "情绪": {
        "importance >= 7": "SHOW_DETAILED",  # 详写生理反应
        "importance 4-6": "SHOW_BRIEF",      # 简写生理反应
        "importance < 4": "TELL"              # 直接说
    },
    "动作": "SHOW_MOSTLY",     # 动作以Show为主
    "信息": "TELL_OK",         # 信息可以Tell
    "环境": "SHOW_SELECTIVELY" # 环境选择性Show
}
```

---

## 【模块1】MAIN_EXECUTION_LOOP - 主执行流程（改进版）

```python
FUNCTION MAIN_EXECUTION(CAPSULE):
    """
    主执行流程：集成实时轻量检查 + 最终全局诊断
    """
    
    # ========== STEP 1: 解析胶囊 ==========
    PRINT "[SYSTEM] 开始解析信息胶囊..."
    parsed_data = PARSE_CAPSULE(CAPSULE)
    VALIDATE_CAPSULE_INTEGRITY(parsed_data)
    
    # ========== STEP 2: 生成任务清单（核心改进）==========
    PRINT "[SYSTEM] 生成拟人化任务清单..."
    humanizer = INIT_HUMANIZE_ENGINE_V3(parsed_data)
    # 返回明确的任务清单，而非概率配置
    
    # ========== STEP 3: 初始化实时监控器 ==========
    monitors = {
        "word_count": 0,
        "last_info_point": 0,
        "last_conflict": 0,
        "scene_checks": []  # 每个场景的轻量检查结果
    }
    
    # ========== STEP 4: 分场景写作（集成轻量检查）==========
    chapter_content = ""
    scene_count = GET_SCENE_COUNT_FROM_CAPSULE(parsed_data)
    
    FOR scene_idx = 1 TO scene_count:
        PRINT "[WRITING] 正在写作场景 {scene_idx}/{scene_count}..."
        
        # 4.1 写作场景
        scene_text = WRITE_SCENE_V3(
            scene_idx, 
            parsed_data, 
            humanizer, 
            chapter_content
        )
        
        chapter_content += scene_text
        monitors.word_count += LENGTH(scene_text)
        
        # 4.2 实时轻量检查（关键改进）
        check_result = LIGHTWEIGHT_SCENE_CHECK(
            scene_text, 
            scene_idx, 
            parsed_data, 
            monitors
        )
        
        monitors.scene_checks.APPEND(check_result)
        
        # 4.3 紧急干预（仅处理严重问题）
        IF check_result.severity == "CRITICAL":
            PRINT "[EMERGENCY] 场景{scene_idx}发现严重问题，立即重写..."
            scene_text = REWRITE_SCENE_WITH_FIX(
                scene_idx, 
                parsed_data, 
                humanizer, 
                check_result.issue
            )
            chapter_content = REPLACE_LAST_SCENE(chapter_content, scene_text)
        END IF
        
        # 4.4 字数预警
        IF monitors.word_count > WORD_COUNT_MAX * 0.85:
            PRINT "[WARNING] 字数接近上限，准备收尾..."
            humanizer.force_ending = TRUE
        END IF
    END FOR
    
    # ========== STEP 5: 集中反Claude病润色（关键改进）==========
    PRINT "[SYSTEM] 执行集中反Claude病润色..."
    chapter_content = APPLY_ALL_ANTI_DISEASE_RULES(chapter_content, parsed_data)
    
    # ========== STEP 6: 全局诊断 ==========
    PRINT "[SYSTEM] 开始全局诊断..."
    diagnosis = DIAGNOSE_CHAPTER_V3(chapter_content, parsed_data, monitors)
    
    # ========== STEP 7: 修正或重写（加入问题分析）==========
    IF diagnosis.has_critical_issues:
        IF NOT EXISTS(humanizer, "rewrite_count"):
            humanizer.rewrite_count = 0
        END IF
        
        IF humanizer.rewrite_count < MAX_REWRITE_ATTEMPTS:
            # 关键改进：加入问题分析
            PRINT "[SYSTEM] 分析问题原因..."
            problem_analysis = ANALYZE_WHAT_WENT_WRONG(diagnosis, parsed_data)
            
            PRINT "[SYSTEM] 生成修正指令..."
            fix_instruction = GENERATE_FIX_INSTRUCTION(problem_analysis)
            
            humanizer.rewrite_count += 1
            PRINT "[SYSTEM] 第{humanizer.rewrite_count}次重写（针对性修正）..."
            
            # 递归重写，但带上修正指令
            RETURN MAIN_EXECUTION_WITH_FIX(CAPSULE, fix_instruction, humanizer)
        ELSE:
            PRINT "[ERROR] 已达最大重写次数，强制交付"
            diagnosis.forced_delivery = TRUE
        END IF
    END IF
    
    # ========== STEP 8: 提取Fact ==========
    new_facts = EXTRACT_FACTS(chapter_content, parsed_data)
    
    # ========== STEP 9: 交付 ==========
    RETURN DELIVER_OUTPUT_V3(chapter_content, new_facts, diagnosis, monitors)
END FUNCTION
```

---

## 【模块2】HUMANIZE_ENGINE_V3 - 拟人化引擎（核心改进）

```python
FUNCTION INIT_HUMANIZE_ENGINE_V3(parsed_data):
    """
    拟人化引擎v3：从概率机制改为任务清单机制
    核心改进：明确告诉Claude"哪个场景必须做什么"
    """
    
    humanizer = {
        "task_checklist": [],    # 任务清单（核心）
        "scene_budget": {},      # 每个场景的字数预算
        "cool_point_plan": {},   # 爽点计划
        "foreshadow_plan": {},   # 伏笔计划
        "rewrite_count": 0,
        "force_ending": FALSE
    }
    
    # ========== 生成任务清单 ==========
    scene_count = ESTIMATE_SCENE_COUNT(parsed_data)
    
    FOR scene_idx = 1 TO scene_count:
        tasks = []
        
        # 任务1：开篇方式（明确指定）
        IF scene_idx == 1:
            tasks.APPEND({
                "type": "开篇",
                "action": RANDOM_CHOICE(["动作直入", "对话直入", "感官直入", "冲突直入"]),
                "priority": "MUST"
            })
        END IF
        
        # 任务2：跳跃式切入（30%场景）
        IF scene_idx > 1 AND scene_idx <= scene_count * 0.3:
            tasks.APPEND({
                "type": "跳跃切入",
                "action": "删除场景前3句铺垫，直接从动作/对话开始",
                "priority": "MUST"
            })
        END IF
        
        # 任务3：故意省略（选择2-3个场景）
        IF scene_idx IN RANDOM_SAMPLE(RANGE(1, scene_count), 2):
            tasks.APPEND({
                "type": "故意省略",
                "action": "某个动机不解释，让读者猜",
                "priority": "SHOULD"
            })
        END IF
        
        # 任务4：笔墨失衡（选择1-2个场景）
        IF scene_idx IN RANDOM_SAMPLE(RANGE(2, scene_count), 2):
            tasks.APPEND({
                "type": "笔墨失衡",
                "action": "某个次要角色/物品突然多写50字",
                "priority": "SHOULD"
            })
        END IF
        
        # 任务5：对话不回应（场景内任务）
        IF RANDOM() < 0.25:  # 25%场景
            tasks.APPEND({
                "type": "对话不回应",
                "action": "至少有1处：A说话，B不接茬，直接做别的",
                "priority": "SHOULD"
            })
        END IF
        
        humanizer.task_checklist[scene_idx] = tasks
    END FOR
    
    # ========== 分配场景字数预算 ==========
    total_budget = WORD_COUNT_TARGET
    scene_importances = GET_SCENE_IMPORTANCES(parsed_data, scene_count)
    
    FOR scene_idx = 1 TO scene_count:
        importance = scene_importances[scene_idx]
        # 重要场景多给字数
        humanizer.scene_budget[scene_idx] = total_budget * (importance / SUM(scene_importances))
    END FOR
    
    # ========== 爽点计划 ==========
    # 从Capsule直接读取（如果有），否则自动规划
    IF parsed_data.goals.cool_point_plan EXISTS:
        humanizer.cool_point_plan = parsed_data.goals.cool_point_plan
    ELSE:
        # 自动规划：平均每1500字一个爽点
        cool_point_count = WORD_COUNT_TARGET / 1500
        cool_point_scenes = DISTRIBUTE_EVENLY(RANGE(1, scene_count), cool_point_count)
        
        FOR scene_idx IN cool_point_scenes:
            suitable_type = SELECT_COOL_POINT_TYPE_FOR_SCENE(scene_idx, parsed_data)
            humanizer.cool_point_plan[scene_idx] = suitable_type
        END FOR
    END IF
    
    # ========== 伏笔计划 ==========
    # 从§16读取，明确显眼度
    FOR foreshadow IN parsed_data.goals.foreshadowing:
        scene_idx = CHOOSE_SCENE_FOR_FORESHADOW(foreshadow, scene_count)
        humanizer.foreshadow_plan[scene_idx] = {
            "content": foreshadow.content,
            "visibility": foreshadow.visibility  # 直接从Capsule指定
        }
    END FOR
    
    PRINT "[HUMANIZE] 任务清单生成完成"
    PRINT "[HUMANIZE] 场景数量: {scene_count}"
    PRINT "[HUMANIZE] 爽点计划: {humanizer.cool_point_plan}"
    
    RETURN humanizer
END FUNCTION
```

---

## 【模块3】LIGHTWEIGHT_SCENE_CHECK - 实时轻量检查（新增）

```python
FUNCTION LIGHTWEIGHT_SCENE_CHECK(scene_text, scene_idx, parsed_data, monitors):
    """
    场景写完后的轻量检查
    只检查最关键的问题，避免过度检查
    """
    
    check_result = {
        "scene_idx": scene_idx,
        "severity": "OK",  # OK / WARNING / CRITICAL
        "issue": NULL,
        "word_count": LENGTH(scene_text)
    }
    
    # ========== 检查1：红线违反（CRITICAL）==========
    FOR redline IN parsed_data.constraints.redlines:
        IF CHECK_REDLINE_VIOLATION(scene_text, redline):
            check_result.severity = "CRITICAL"
            check_result.issue = f"违反红线：{redline}"
            RETURN check_result
        END IF
    END FOR
    
    # ========== 检查2：字数严重超标（CRITICAL）==========
    IF LENGTH(scene_text) > 1200:  # 单场景超过1200字
        check_result.severity = "CRITICAL"
        check_result.issue = f"场景{scene_idx}字数过多({LENGTH(scene_text)}字)，会导致全章超标"
        RETURN check_result
    END IF
    
    # ========== 检查3：信息密度过低（WARNING）==========
    info_density = COUNT_NEW_INFO(scene_text) / LENGTH(scene_text)
    IF info_density < TOMATO_CORE_RULES.info_density_min:
        check_result.severity = "WARNING"
        check_result.issue = f"场景{scene_idx}信息密度过低({info_density})"
        # WARNING不触发重写，仅记录
    END IF
    
    # ========== 检查4：冲突缺失（WARNING）==========
    words_since_conflict = monitors.word_count - monitors.last_conflict
    IF words_since_conflict > 600:
        check_result.severity = "WARNING"
        check_result.issue = f"已连续{words_since_conflict}字无冲突"
    END IF
    
    RETURN check_result
END FUNCTION
```

---

## 【模块4】WRITE_SCENE_V3 - 场景写作器（简化版）

```python
FUNCTION WRITE_SCENE_V3(scene_idx, parsed_data, humanizer, previous_content):
    """
    写作单个场景（v3简化版）
    核心改进：执行任务清单，而非概率决策
    """
    
    scene_text = ""
    scene_budget = humanizer.scene_budget[scene_idx]
    tasks = humanizer.task_checklist[scene_idx]
    
    # ========== STEP 1: 执行开篇任务 ==========
    opening_task = FIND_TASK(tasks, "开篇")
    IF opening_task:
        scene_text = EXECUTE_OPENING_TASK(opening_task, parsed_data)
    ELSE:
        scene_text = WRITE_DEFAULT_OPENING(scene_idx, parsed_data)
    END IF
    
    # ========== STEP 2: 写作场景主体 ==========
    # 将场景拆解为写作单元
    units = DECOMPOSE_SCENE_TO_UNITS(scene_idx, parsed_data)
    
    FOR unit IN units:
        # 2.1 字数控制
        IF LENGTH(scene_text) > scene_budget * 0.9:
            BREAK  # 接近预算，停止
        END IF
        
        # 2.2 根据重要性决定展开程度
        expansion_level = DECIDE_EXPANSION_BY_IMPORTANCE(unit.importance)
        
        # 2.3 写作单元
        unit_text = WRITE_UNIT_BY_TYPE(
            unit, 
            expansion_level, 
            parsed_data
        )
        
        scene_text += unit_text
    END FOR
    
    # ========== STEP 3: 执行拟人化任务 ==========
    FOR task IN tasks:
        IF task.type != "开篇":  # 开篇任务已执行
            scene_text = EXECUTE_HUMANIZE_TASK(scene_text, task)
        END IF
    END FOR
    
    # ========== STEP 4: 注入爽点（如果计划中有）==========
    IF scene_idx IN humanizer.cool_point_plan:
        cool_type = humanizer.cool_point_plan[scene_idx]
        cool_text = GENERATE_COOL_POINT(cool_type, parsed_data)
        scene_text = INSERT_COOL_POINT(scene_text, cool_text)
    END IF
    
    # ========== STEP 5: 埋伏笔（如果计划中有）==========
    IF scene_idx IN humanizer.foreshadow_plan:
        foreshadow = humanizer.foreshadow_plan[scene_idx]
        foreshadow_text = WRITE_FORESHADOW(foreshadow, parsed_data)
        scene_text = INSERT_FORESHADOW(scene_text, foreshadow_text)
    END IF
    
    # ========== STEP 6: 场景结尾 ==========
    ending = WRITE_SCENE_ENDING(scene_idx, parsed_data, humanizer)
    scene_text += ending
    
    RETURN scene_text
END FUNCTION

# ==================== 辅助函数 ====================

FUNCTION DECIDE_EXPANSION_BY_IMPORTANCE(importance):
    """
    根据重要性决定展开程度（规则化）
    """
    IF importance >= 8:
        RETURN "EXPAND"      # 展开50-100字
    ELSE IF importance >= 5:
        RETURN "BRIEF"       # 简写20-50字
    ELSE:
        RETURN "SKIP"        # 一句话带过或跳过
    END IF
END FUNCTION

FUNCTION WRITE_UNIT_BY_TYPE(unit, expansion_level, parsed_data):
    """
    根据单元类型写作
    """
    SWITCH unit.type:
        CASE "动作":
            RETURN WRITE_ACTION(unit, expansion_level, parsed_data)
        CASE "对话":
            RETURN WRITE_DIALOGUE(unit, expansion_level, parsed_data)
        CASE "情绪":
            RETURN WRITE_EMOTION(unit, expansion_level, parsed_data)
        CASE "描写":
            RETURN WRITE_DESCRIPTION(unit, expansion_level, parsed_data)
        CASE "内心戏":
            IF expansion_level == "EXPAND":
                RETURN WRITE_INNER_MONOLOGUE(unit, parsed_data)
            ELSE:
                RETURN ""  # 低重要度的内心戏直接跳过
            END IF
    END SWITCH
END FUNCTION

FUNCTION EXECUTE_HUMANIZE_TASK(scene_text, task):
    """
    执行拟人化任务
    """
    SWITCH task.type:
        CASE "跳跃切入":
            # 删除前3句
            RETURN REMOVE_FIRST_N_SENTENCES(scene_text, 3)
        
        CASE "故意省略":
            # 找到一个动机描述，删除
            RETURN REMOVE_ONE_MOTIVATION(scene_text)
        
        CASE "笔墨失衡":
            # 找到一个次要元素，多写50字
            RETURN EXPAND_MINOR_ELEMENT(scene_text, 50)
        
        CASE "对话不回应":
            # 已在对话写作时处理
            RETURN scene_text
        
        DEFAULT:
            RETURN scene_text
    END SWITCH
END FUNCTION
```

---

## 【模块5】ANTI_DISEASE_PROTOCOL_V3 - 集中反Claude病（关键改进）

```python
FUNCTION APPLY_ALL_ANTI_DISEASE_RULES(chapter_content, parsed_data):
    """
    集中应用所有反Claude病规则
    在润色阶段统一处理，而非分散在写作过程中
    """
    
    PRINT "[POLISH] 开始集中反Claude病润色..."
    
    polished = chapter_content
    
    # ========== 病症1：删除口水词 ==========
    polished = REMOVE_FILLER_WORDS(polished)
    # 删除：似乎/仿佛/或许/可能/大概/好像
    
    # ========== 病症2：删除套路转折词 ==========
    polished = REMOVE_CLICHE_TRANSITIONS(polished)
    # 删除：然而/但是/就在这时/突然/忽然
    
    # ========== 病症3：替换情绪词为生理反应 ==========
    polished = REPLACE_EMOTION_WORDS_WITH_PHYSIOLOGY(polished)
    # "他感到恐惧" → "他的后背发凉"
    
    # ========== 病症4：简化动作链 ==========
    polished = SIMPLIFY_ACTION_CHAINS(polished)
    # "站起来→走到门口→推门→走出去" → "他推门出去"
    
    # ========== 病症5：删除段落总结句 ==========
    polished = REMOVE_PARAGRAPH_SUMMARIES(polished)
    # 删除"他明白了""他决定了""他意识到"结尾
    
    # ========== 病症6：压缩过长段落 ==========
    polished = COMPRESS_LONG_PARAGRAPHS(polished, MAX_LENGTH=150)
    # 单段>150字的，提取核心重写
    
    # ========== 病症7：减少内心戏 ==========
    polished = REDUCE_INNER_MONOLOGUE(polished, MAX_RATIO=0.15)
    # 内心戏>15%的，删减到15%
    
    # ========== 病症8：删除冗余 ==========
    polished = REMOVE_REDUNDANCY(polished)
    # 同一意思说2-3遍的，只保留1遍
    
    # ========== 病症9：优化句长（长短句交错）==========
    polished = OPTIMIZE_SENTENCE_LENGTH(polished)
    # 避免连续5句都是20字以上或10字以下
    
    # ========== 病症10：删除解释性内心戏 ==========
    polished = REMOVE_EXPLANATORY_THOUGHTS(polished)
    # "他这样做是因为..." → 直接删除，让读者自己推断
    
	
	
	
	
	
	# ========== 病症11：过度安全（追加）==========
    polished = REMOVE_HEDGING_LANGUAGE(polished)
    # 删除过度谨慎的表达："可以说""基本上""在某种意义上"
    
    # ========== 病症12：过度冗余（追加）==========
    polished = REMOVE_REPETITION(polished)
    # 检测同一意思用不同方式重复2-3遍的段落，只保留1遍
    
    # ========== 病症13：过度铺垫（追加）==========
    polished = COMPRESS_SETUP(polished)
    # 检测事件前超过100字的铺垫，压缩到30-50字
    
    # ========== 病症14：不敢跳跃（追加）==========
    polished = REMOVE_TRANSITIONS(polished)
    # 删除"过了X时间""他走到了Y地"等过渡句
    
    # ========== 病症15：过度平均（追加）==========
    # 注：此项在写作阶段已通过"笔墨失衡"任务处理，润色不处理
    
    # ========== 病症16：过度逻辑（追加）==========
    polished = REMOVE_CAUSALITY_EXPLANATION(polished)
    # 删除"因为X所以Y""之所以X是因为Y"等解释性连接
    
    # ========== 病症17：过度完整（追加）==========
    polished = BREAK_SCENE_STRUCTURE(polished)
    # 识别"起承转合"完整的场景，随机删除"承"或"转"
    
    # ========== 病症18：过度礼貌（追加）==========
    polished = ADD_CONVERSATIONAL_ROUGHNESS(polished)
    # 在对话中添加省略、打断、单字回应
    
    # ========== 病症19：过度对称（追加）==========
    # 注：此项在写作阶段已通过"对话不回应"任务处理，润色不处理
    
    # ========== 病症20：信息密度不足（追加）==========
    polished = COMPRESS_LOW_DENSITY_PARAGRAPHS(polished)
    # 检测信息密度<0.005的段落，压缩或删除
    
    PRINT "[POLISH] 反Claude病润色完成（20条规则已应用）"
    
    RETURN polished
END FUNCTION
	
	
	
	


# ==================== 具体实现函数（示例）====================

FUNCTION REMOVE_FILLER_WORDS(text):
    """删除口水词"""
    filler_words = ["似乎", "仿佛", "或许", "可能", "大概", "好像", "某种程度上"]
    FOR word IN filler_words:
        text = REPLACE(text, word, "")
    END FOR
    RETURN CLEAN_WHITESPACE(text)
END FUNCTION

FUNCTION REPLACE_EMOTION_WORDS_WITH_PHYSIOLOGY(text):
    """替换情绪词为生理反应"""
    emotion_map = {
        "震惊": ["瞳孔骤然收缩", "呼吸一滞", "僵在原地"],
        "恐惧": ["后背发凉", "腿发软", "冷汗", "心跳骤停"],
        "愤怒": ["太阳穴跳动", "拳头攥紧", "脸颊发烫"],
        "焦虑": ["胃部收紧", "手心冒汗", "呼吸变浅"],
        "紧张": ["喉咙发干", "心跳加速", "肌肉紧绷"]
    }
    
    FOR emotion_word, reactions IN emotion_map.ITEMS():
        # 找到"他感到XX"的模式
        pattern = f"(他|她).*感到{emotion_word}"
        matches = FIND_ALL(text, pattern)
        
        FOR match IN matches:
            # 替换为随机选择的生理反应
            reaction = RANDOM_CHOICE(reactions)
            subject = EXTRACT_SUBJECT(match)  # "他"或"她"
            replacement = f"{subject}的{reaction}"
            text = REPLACE(text, match, replacement)
        END FOR
    END FOR
    
    RETURN text
END FUNCTION

FUNCTION SIMPLIFY_ACTION_CHAINS(text):
    """简化动作链"""
    # 检测动作链模式：连续4个以上动作
    pattern = r"(他|她)(\w{2,5})(，|然后|接着)(\w{2,5})(，|然后|接着)(\w{2,5})(，|然后|接着)(\w{2,5})"
    
    matches = FIND_ALL(text, pattern)
    
    FOR match IN matches:
        actions = EXTRACT_ACTIONS(match)  # 提取所有动作
        key_action = SELECT_KEY_ACTION(actions)  # 选择关键动作
        subject = EXTRACT_SUBJECT(match)  # "他"或"她"
        
        simplified = f"{subject}{key_action}"
        text = REPLACE(text, match.full_text, simplified)
    END FOR
    
    RETURN text
END FUNCTION


# ==================== 新增病症处理函数（D11-D20）====================

FUNCTION REMOVE_HEDGING_LANGUAGE(text):
    """
    D4/D11：删除过度谨慎的表达
    """
    hedging_phrases = [
        "可以说", "基本上", "在某种意义上", "从某种角度", 
        "相对而言", "总的来说", "一定程度上"
    ]
    FOR phrase IN hedging_phrases:
        text = REPLACE(text, phrase, "")
    END FOR
    RETURN CLEAN_WHITESPACE(text)
END FUNCTION

FUNCTION REMOVE_REPETITION(text):
    """
    D5/D12：删除同一意思的重复表达
    """
    paragraphs = SPLIT_PARAGRAPHS(text)
    
    FOR i, para IN ENUMERATE(paragraphs):
        sentences = SPLIT_SENTENCES(para)
        
        # 检测相邻3句是否表达相同意思
        FOR j = 0 TO LENGTH(sentences) - 3:
            IF ARE_SEMANTICALLY_SIMILAR(sentences[j], sentences[j+1], sentences[j+2]):
                # 删除后两句，只保留第一句
                sentences = DELETE(sentences, [j+1, j+2])
            END IF
        END FOR
        
        paragraphs[i] = JOIN(sentences, "")
    END FOR
    
    RETURN JOIN(paragraphs, "\n\n")
END FUNCTION

FUNCTION COMPRESS_SETUP(text):
    """
    D14：压缩过度铺垫
    """
    paragraphs = SPLIT_PARAGRAPHS(text)
    
    FOR i = 0 TO LENGTH(paragraphs) - 2:
        current = paragraphs[i]
        next = paragraphs[i+1]
        
        # 检测铺垫模式：当前段落>100字 + 下一段落包含动作词
        IF LENGTH(current) > 100 AND CONTAINS_ACTION_TRIGGER(next):
            # 压缩当前段落到30-50字
            core = EXTRACT_CORE_INFO(current)
            paragraphs[i] = REWRITE_BRIEFLY(core, MAX_LENGTH=50)
        END IF
    END FOR
    
    RETURN JOIN(paragraphs, "\n\n")
END FUNCTION

FUNCTION REMOVE_TRANSITIONS(text):
    """
    D17：删除过渡句
    """
    transition_patterns = [
        r"过了.{1,5}(时间|分钟|小时|天)",
        r"(他|她).{0,5}(走到|来到|到了).{2,10}",
        r"片刻之后",
        r"不多时",
        r"须臾"
    ]
    
    FOR pattern IN transition_patterns:
        matches = FIND_ALL(text, pattern)
        FOR match IN matches:
            # 检查是否是独立的过渡句（前后都有换行）
            IF IS_STANDALONE_SENTENCE(text, match):
                text = REPLACE(text, match.full_sentence, "")
            END IF
        END FOR
    END FOR
    
    RETURN CLEAN_WHITESPACE(text)
END FUNCTION

FUNCTION REMOVE_CAUSALITY_EXPLANATION(text):
    """
    D7/D16：删除因果解释
    """
    causality_patterns = [
        r"因为.{5,30}所以",
        r"之所以.{5,30}是因为",
        r"这(是|样做)(是)因为",
        r"正(是|)由于"
    ]
    
    FOR pattern IN causality_patterns:
        matches = FIND_ALL(text, pattern)
        FOR match IN matches:
            # 保留"所以"后面的内容，删除"因为"部分
            result_part = EXTRACT_RESULT_CLAUSE(match)
            text = REPLACE(text, match.full_text, result_part)
        END FOR
    END FOR
    
    RETURN text
END FUNCTION

FUNCTION BREAK_SCENE_STRUCTURE(text):
    """
    D8/D17：打破场景完整结构
    """
    scenes = DETECT_SCENES(text)
    
    FOR scene IN scenes:
        structure = ANALYZE_SCENE_STRUCTURE(scene)
        
        # 如果场景有完整的"起承转合"
        IF structure.has_all_four_parts:
            # 随机删除"承"或"转"（30%概率）
            IF RANDOM() < 0.3:
                part_to_remove = RANDOM_CHOICE(["承", "转"])
                scene.content = REMOVE_PART(scene.content, part_to_remove)
                text = REPLACE_SCENE(text, scene)
            END IF
        END IF
    END FOR
    
    RETURN text
END FUNCTION

FUNCTION ADD_CONVERSATIONAL_ROUGHNESS(text):
    """
    D9/D18：增加对话的"粗糙感"
    """
    dialogues = EXTRACT_ALL_DIALOGUES(text)
    
    FOR dialogue IN dialogues:
        # 30%概率做以下处理之一
        IF RANDOM() < 0.3:
            modification = RANDOM_CHOICE([
                "TRUNCATE",      # 说半句话
                "SINGLE_CHAR",   # 改成"嗯""啊""哦"
                "ADD_PAUSE"      # 添加"……"
            ])
            
            SWITCH modification:
                CASE "TRUNCATE":
                    # "你这是什么意思？" → "你这是什么——"
                    dialogue.content = TRUNCATE_AT_RANDOM(dialogue.content)
                
                CASE "SINGLE_CHAR":
                    # 完整回答 → "嗯"
                    IF LENGTH(dialogue.content) > 10 AND IS_RESPONSE(dialogue):
                        dialogue.content = RANDOM_CHOICE(["嗯", "啊", "哦", "呵"])
                    END IF
                
                CASE "ADD_PAUSE":
                    # 添加犹豫停顿
                    dialogue.content = INSERT_PAUSE(dialogue.content)
            END SWITCH
            
            text = REPLACE_DIALOGUE(text, dialogue)
        END IF
    END FOR
    
    RETURN text
END FUNCTION

FUNCTION COMPRESS_LOW_DENSITY_PARAGRAPHS(text):
    """
    D11/D20：压缩低信息密度段落
    """
    paragraphs = SPLIT_PARAGRAPHS(text)
    
    FOR i, para IN ENUMERATE(paragraphs):
        density = CALCULATE_INFO_DENSITY(para)
        
        # 信息密度<0.005（每200字不到1个信息点）
        IF density < 0.005 AND LENGTH(para) > 100:
            # 提取核心信息，重写为简短版本
            core = EXTRACT_CORE_INFO(para)
            
            IF LENGTH(core) < 20:
                # 核心信息太少，直接删除
                paragraphs[i] = ""
            ELSE:
                # 压缩到30-50字
                paragraphs[i] = REWRITE_BRIEFLY(core, MAX_LENGTH=50)
            END IF
        END IF
    END FOR
    
    RETURN JOIN(FILTER(paragraphs, NOT_EMPTY), "\n\n")
END FUNCTION

# ==================== 辅助检测函数 ====================

FUNCTION ARE_SEMANTICALLY_SIMILAR(sent1, sent2, sent3):
    """判断3句话是否语义相似（简化实现）"""
    # 提取关键词，计算重叠度
    keywords1 = EXTRACT_KEYWORDS(sent1)
    keywords2 = EXTRACT_KEYWORDS(sent2)
    keywords3 = EXTRACT_KEYWORDS(sent3)
    
    overlap_12 = LENGTH(INTERSECT(keywords1, keywords2)) / LENGTH(UNION(keywords1, keywords2))
    overlap_23 = LENGTH(INTERSECT(keywords2, keywords3)) / LENGTH(UNION(keywords2, keywords3))
    
    RETURN overlap_12 > 0.6 AND overlap_23 > 0.6
END FUNCTION

FUNCTION CONTAINS_ACTION_TRIGGER(text):
    """检测是否包含动作触发词"""
    action_triggers = ["突然", "这时", "就在", "只见", "忽然", "猛地"]
    RETURN ANY(trigger IN text FOR trigger IN action_triggers)
END FUNCTION

FUNCTION IS_STANDALONE_SENTENCE(text, match):
    """判断匹配是否是独立句子"""
    start = match.start_pos
    end = match.end_pos
    
    # 检查前后是否有句号/换行
    before_char = text[start-1] IF start > 0 ELSE "\n"
    after_char = text[end] IF end < LENGTH(text) ELSE "\n"
    
    RETURN before_char IN ["。", "\n"] AND after_char IN ["。", "\n"]
END FUNCTION

FUNCTION CALCULATE_INFO_DENSITY(paragraph):
    """计算段落信息密度"""
    info_count = COUNT_NEW_INFO(paragraph)
    char_count = LENGTH(paragraph)
    RETURN info_count / char_count
END FUNCTION




```

---

## 【模块6】DIAGNOSE_CHAPTER_V3 - 全局诊断（优化版）

```python
FUNCTION DIAGNOSE_CHAPTER_V3(chapter_content, parsed_data, monitors):
    """
    全局诊断（v3优化版）
    输出分层报告，快速定位问题
    """
    
    diagnosis = {
        "passed": TRUE,
        "summary": {},      # 快速摘要
        "critical": [],     # 必须修复
        "warnings": [],     # 建议优化
        "details": {}       # 详细数据
    }
    
    # ========== 计算统计数据 ==========
    stats = {
        "word_count": LENGTH(chapter_content),
        "dialogue_ratio": CALCULATE_DIALOGUE_RATIO(chapter_content),
        "inner_ratio": CALCULATE_INNER_MONOLOGUE_RATIO(chapter_content),
        "paragraph_count": COUNT_PARAGRAPHS(chapter_content),
        "avg_para_length": AVG_PARAGRAPH_LENGTH(chapter_content),
        "info_density": CALCULATE_INFO_DENSITY(chapter_content),
        "disease_count": COUNT_CLAUDE_DISEASES_V3(chapter_content),
        "scene_checks": monitors.scene_checks  # 实时检查结果
    }
    
    # ========== 快速摘要 ==========
    diagnosis.summary = {
        "status": "✅ 通过" IF diagnosis.passed ELSE "❌ 未通过",
        "word_count": f"{stats.word_count}字",
        "quality_score": CALCULATE_QUALITY_SCORE(stats)  # 0-100
    }
    
    # ========== 关键检查 ==========
    
    # 检查1：字数范围
    IF stats.word_count < WORD_COUNT_MIN:
        diagnosis.critical.APPEND({
            "issue": f"字数不足（{stats.word_count}字，最低{WORD_COUNT_MIN}字）",
            "fix": "需要扩充关键情节或增加冲突"
        })
        diagnosis.passed = FALSE
    END IF
    
    # 检查2：核心任务
    core_mission = parsed_data.goals.core_mission
    IF NOT CHECK_MISSION_COMPLETED(chapter_content, core_mission):
        diagnosis.critical.APPEND({
            "issue": f"核心任务未完成：{core_mission}",
            "fix": "必须重写或补充关键情节"
        })
        diagnosis.passed = FALSE
    END IF
    
    # 检查3：红线违反
    FOR redline IN parsed_data.constraints.redlines:
        IF CHECK_REDLINE_VIOLATION(chapter_content, redline):
            diagnosis.critical.APPEND({
                "issue": f"违反红线：{redline}",
                "fix": "必须删除违规内容"
            })
            diagnosis.passed = FALSE
        END IF
    END FOR
    
    # 检查4：必须埋的钩子
    FOR hook IN parsed_data.goals.hooks_to_plant:
        IF NOT CHECK_HOOK_PLANTED(chapter_content, hook):
            diagnosis.critical.APPEND({
                "issue": f"钩子未埋：{hook}",
                "fix": "需要补充伏笔"
            })
            # 钩子未埋不算CRITICAL，但要标记
        END IF
    END FOR
    
    # ========== 警告项 ==========
    
    # 警告1：对话占比
    dialogue_ratio = stats.dialogue_ratio
    target_range = TOMATO_CORE_RULES.dialogue_ratio
    IF dialogue_ratio < target_range[0] OR dialogue_ratio > target_range[1]:
        diagnosis.warnings.APPEND({
            "issue": f"对话占比偏离目标（当前{dialogue_ratio*100:.1f}%，目标{target_range[0]*100}-{target_range[1]*100}%）",
            "fix": "调整对话/描写比例"
        })
    END IF
    
    # 警告2：Claude病症
    IF stats.disease_count > 5:
        diagnosis.warnings.APPEND({
            "issue": f"检出{stats.disease_count}个Claude病症",
            "fix": "已自动润色，但仍需人工检查"
        })
    END IF
    
    # 警告3：信息密度
    IF stats.info_density < TOMATO_CORE_RULES.info_density_min:
        diagnosis.warnings.APPEND({
            "issue": f"信息密度过低（{stats.info_density:.3f}）",
            "fix": "压缩描写或增加信息点"
        })
    END IF
    
    # ========== 详细数据 ==========
    diagnosis.details = stats
    
    RETURN diagnosis
END FUNCTION

FUNCTION CALCULATE_QUALITY_SCORE(stats):
    """
    计算综合质量分（0-100）
    """
    score = 100
    
    # 字数：在目标范围内+20，超出-20
    IF stats.word_count < WORD_COUNT_MIN OR stats.word_count > WORD_COUNT_MAX:
        score -= 20
    ELSE IF ABS(stats.word_count - WORD_COUNT_TARGET) < 500:
        score += 10
    END IF
    
    # 对话占比：在目标范围内+20
    IF stats.dialogue_ratio >= 0.35 AND stats.dialogue_ratio <= 0.45:
        score += 15
    ELSE:
        score -= 10
    END IF
    
    # 信息密度：达标+15
    IF stats.info_density >= TOMATO_CORE_RULES.info_density_min:
        score += 15
    ELSE:
        score -= 10
    END IF
    
    # Claude病症：每个-3分
    score -= stats.disease_count * 3
    
    # 段落长度：平均<150字+10分
    IF stats.avg_para_length <= 150:
        score += 10
    END IF
    
    RETURN CLAMP(score, 0, 100)
END FUNCTION
```

---

## 【模块7】PROBLEM_ANALYSIS - 问题分析（新增）

```python
FUNCTION ANALYZE_WHAT_WENT_WRONG(diagnosis, parsed_data):
    """
    分析诊断失败的根本原因
    避免盲目重写
    """
    
    analysis = {
        "root_cause": "",
        "affected_scenes": [],
        "fix_strategy": ""
    }
    
    # ========== 分析Critical问题 ==========
    FOR issue IN diagnosis.critical:
        IF "字数不足" IN issue.issue:
            analysis.root_cause = "内容量不足"
            analysis.fix_strategy = "展开重要情节，增加冲突强度"
            
        ELSE IF "核心任务未完成" IN issue.issue:
            analysis.root_cause = "遗漏关键情节"
            analysis.fix_strategy = "补充任务相关的场景"
            
        ELSE IF "违反红线" IN issue.issue:
            # 从章节内容中找到违规位置
            violation_location = FIND_VIOLATION_LOCATION(chapter_content, issue)
            analysis.affected_scenes = [violation_location.scene_idx]
            analysis.root_cause = "逻辑错误或理解偏差"
            analysis.fix_strategy = f"删除场景{violation_location.scene_idx}的违规内容"
            
        ELSE IF "钩子未埋" IN issue.issue:
            analysis.root_cause = "遗漏伏笔"
            analysis.fix_strategy = "在适当场景补充伏笔"
        END IF
    END FOR
    
    # ========== 分析Warning（辅助判断）==========
    IF LENGTH(diagnosis.warnings) > 3:
        analysis.root_cause += " + 风格偏离"
        analysis.fix_strategy += " + 调整节奏和比例"
    END IF
    
    PRINT "[ANALYSIS] 问题根源：{analysis.root_cause}"
    PRINT "[ANALYSIS] 修复策略：{analysis.fix_strategy}"
    
    RETURN analysis
END FUNCTION

FUNCTION GENERATE_FIX_INSTRUCTION(problem_analysis):
    """
    根据问题分析生成修正指令
    """
    
    fix_instruction = {
        "focus_scenes": problem_analysis.affected_scenes,
        "must_do": [],
        "must_avoid": []
    }
    
    IF "内容量不足" IN problem_analysis.root_cause:
        fix_instruction.must_do.APPEND("展开重要情节至200-300字")
        fix_instruction.must_do.APPEND("增加1-2个冲突场景")
    END IF
    
    IF "遗漏关键情节" IN problem_analysis.root_cause:
        fix_instruction.must_do.APPEND("补充核心任务相关场景")
    END IF
    
    IF "逻辑错误" IN problem_analysis.root_cause:
        fix_instruction.must_avoid.APPEND("避免违反红线的内容")
        fix_instruction.focus_scenes = problem_analysis.affected_scenes
    END IF
    
    IF "风格偏离" IN problem_analysis.root_cause:
        fix_instruction.must_do.APPEND("增加对话占比至35%-45%")
        fix_instruction.must_do.APPEND("控制段落长度<150字")
    END IF
    
    RETURN fix_instruction
END FUNCTION

FUNCTION MAIN_EXECUTION_WITH_FIX(CAPSULE, fix_instruction, humanizer):
    """
    带修正指令的重写
    """
    
    # 在humanizer中注入修正指令
    humanizer.fix_instruction = fix_instruction
    
    # 重新执行主流程
    RETURN MAIN_EXECUTION(CAPSULE)
    
    # 注：写作时会检查humanizer.fix_instruction，
    # 针对focus_scenes重点修正
END FUNCTION
```

---

## 【模块8】EMOTION_WRITER_V3 - 情绪写作器（简化版）

```python
FUNCTION WRITE_EMOTION(unit, expansion_level, parsed_data):
    """
    写情绪单元（v3简化版）
    根据expansion_level动态Show/Tell
    """
    
    emotion_text = ""
    emotion_type = unit.emotion_type  # 如：焦虑
    emotion_intensity = unit.intensity  # 0-100
    
    # ========== 根据expansion_level决定Show/Tell ==========
    IF expansion_level == "EXPAND":
        # 详写：生理反应 + 简短内心戏
        micro_expressions = GENERATE_MICRO_EXPRESSIONS(emotion_type, emotion_intensity)
        emotion_text = WRITE_MICRO_EXPRESSIONS(micro_expressions)
        
        # 加一句内心戏（<30字）
        IF parsed_data.emotions.protagonist.active_thought:
            inner = parsed_data.emotions.protagonist.active_thought
            emotion_text += f""{inner[:30]}""  # 限制30字
        END IF
        
    ELSE IF expansion_level == "BRIEF":
        # 简写：只写生理反应
        micro_expressions = GENERATE_MICRO_EXPRESSIONS(emotion_type, emotion_intensity)
        emotion_text = WRITE_MICRO_EXPRESSIONS(micro_expressions[:2])  # 只取2个
        
    ELSE:  # SKIP
        # 跳过：不写情绪
        emotion_text = ""
    END IF
    
    RETURN emotion_text
END FUNCTION

FUNCTION GENERATE_MICRO_EXPRESSIONS(emotion_type, intensity):
    """生成生理反应"""
    emotion_map = {
        "焦虑": ["胃部收紧", "呼吸变浅", "手心冒汗"],
        "恐惧": ["后背发凉", "腿发软", "瞳孔收缩"],
        "愤怒": ["太阳穴跳动", "拳头攥紧", "脸颊发烫"],
        "兴奋": ["心跳加快", "呼吸急促", "眼睛发亮"]
    }
    
    reactions = emotion_map.GET(emotion_type, ["身体紧绷"])
    
    # 根据强度选择数量
    IF intensity > 70:
        RETURN reactions  # 全部
    ELSE IF intensity > 40:
        RETURN reactions[:2]  # 前2个
    ELSE:
        RETURN reactions[:1]  # 前1个
    END IF
END FUNCTION
```

---

## 【模块9】DELIVERY_V3 - 交付协议（优化版）

```python
FUNCTION DELIVER_OUTPUT_V3(chapter_content, new_facts, diagnosis, monitors):
    """
    交付输出（v3分层版）
    """
    
    output = {}
    
    # ========== 输出1：章节正文 ==========
    output["chapter_content"] = chapter_content
    
    # ========== 输出2：快速摘要 ==========
    output["summary"] = f"""
## 快速摘要
{diagnosis.summary.status} | 字数: {diagnosis.summary.word_count} | 质量分: {diagnosis.summary.quality_score}/100
"""
    
    # ========== 输出3：问题清单（如果有）==========
    IF LENGTH(diagnosis.critical) > 0:
        output["summary"] += "\n### ⚠️ 必须修复\n"
        FOR issue IN diagnosis.critical:
            output["summary"] += f"- {issue.issue}\n"
        END FOR
    END IF
    
    IF LENGTH(diagnosis.warnings) > 0:
        output["summary"] += "\n### 💡 建议优化\n"
        FOR warning IN diagnosis.warnings:
            output["summary"] += f"- {warning.issue}\n"
        END FOR
    END IF
    
    # ========== 输出4：新增Fact清单 ==========
    output["new_facts"] = FORMAT_FACTS_LIST(new_facts)
    
    # ========== 输出5：详细诊断（可折叠）==========
    output["details"] = f"""
<details>
<summary>📊 详细统计（点击展开）</summary>

- 总字数: {diagnosis.details.word_count}
- 对话占比: {diagnosis.details.dialogue_ratio * 100:.1f}%
- 内心戏占比: {diagnosis.details.inner_ratio * 100:.1f}%
- 段落数: {diagnosis.details.paragraph_count}
- 平均段落长度: {diagnosis.details.avg_para_length:.0f}字
- 信息密度: {diagnosis.details.info_density:.3f}
- Claude病症检出: {diagnosis.details.disease_count}个

### 场景检查记录
"""
    FOR check IN monitors.scene_checks:
        status_icon = "✅" IF check.severity == "OK" ELSE "⚠️"
        output["details"] += f"- 场景{check.scene_idx}: {status_icon} {check.word_count}字\n"
    END FOR
    
    output["details"] += "</details>"
    
    RETURN output
END FUNCTION
```

---

## 【附录】辅助函数说明（精简版）

```python
# ==================== 核心辅助函数 ====================

FUNCTION PARSE_CAPSULE(CAPSULE):
    """解析胶囊，提取所有模块数据"""
    # 从§1-§19提取结构化数据
    PASS
END FUNCTION

FUNCTION GET_SCENE_COUNT_FROM_CAPSULE(parsed_data):
    """
    从Capsule推断场景数量
    规则：3000-4000字 → 4-5个场景
    """
    word_target = parsed_data.meta.word_count_target OR WORD_COUNT_TARGET
    RETURN ROUND(word_target / 800)  # 平均每场景800字
END FUNCTION

FUNCTION CHECK_REDLINE_VIOLATION(text, redline):
    """检查是否违反红线（语义匹配）"""
    keywords = EXTRACT_KEYWORDS(redline)
    IF ALL(keyword IN text FOR keyword IN keywords):
        RETURN SEMANTIC_CHECK(text, redline)  # 进一步语义验证
    END IF
    RETURN FALSE
END FUNCTION

FUNCTION COUNT_NEW_INFO(text):
    """统计新信息点数量"""
    # 简化实现：检测关键词、对话、新概念等
    info_indicators = [
        COUNT_PROPER_NOUNS(text),           # 专有名词
        COUNT_PLOT_VERBS(text),             # 剧情动词（发现/得到/失去）
        COUNT_DIALOGUE_EXCHANGES(text) / 2  # 对话回合
    ]
    RETURN SUM(info_indicators)
END FUNCTION

FUNCTION CALCULATE_DIALOGUE_RATIO(text):
    """计算对话占比"""
    dialogue_chars = LENGTH(EXTRACT_ALL_DIALOGUES(text))
    total_chars = LENGTH(text)
    RETURN dialogue_chars / total_chars
END FUNCTION

FUNCTION CHECK_MISSION_COMPLETED(text, mission):
    """检查核心任务是否完成（关键词+语义）"""
    mission_keywords = EXTRACT_KEYWORDS(mission)
    IF ANY(keyword NOT IN text FOR keyword IN mission_keywords):
        RETURN FALSE
    END IF
    # 进一步语义检查
    RETURN SEMANTIC_VERIFY_MISSION(text, mission)
END FUNCTION

FUNCTION CHECK_HOOK_PLANTED(text, hook):
    """检查钩子是否埋下"""
    hook_keywords = EXTRACT_KEYWORDS(hook)
    RETURN ALL(keyword IN text FOR keyword IN hook_keywords)
END FUNCTION

FUNCTION WRITE_FORESHADOW(foreshadow, parsed_data):
    """
    写伏笔（根据显眼度）
    显眼度直接从Capsule§16读取
    """
    visibility = foreshadow.visibility
    content = foreshadow.content
    
    SWITCH visibility:
        CASE "极度隐蔽":
            # 混在5个其他细节中，一笔带过
            other_details = GENERATE_RANDOM_DETAILS(5, parsed_data)
            all_details = SHUFFLE([content] + other_details)
            RETURN JOIN(all_details, "，") + "。"
        
        CASE "隐蔽":
            # 正常描写，不特别强调
            RETURN f"{content}。"
        
        CASE "微显眼":
            # 略微多写几个字
            detail = ADD_MINOR_DETAIL(content)
            RETURN f"{content}，{detail}。"
        
        CASE "显眼":
            # 单独成句
            RETURN f"\n\n{content}。\n\n"
        
        CASE "极度显眼":
            # 强调 + 主角注意到
            reaction = GENERATE_PROTAGONIST_REACTION(content, parsed_data)
            RETURN f"\n\n{content}。{reaction}\n\n"
    END SWITCH
END FUNCTION

FUNCTION SELECT_COOL_POINT_TYPE_FOR_SCENE(scene_idx, parsed_data):
    """为场景选择合适的爽点类型"""
    # 从Capsule读取场景信息
    scene_info = GET_SCENE_INFO(scene_idx, parsed_data)
    
    IF scene_info.has_conflict:
        RETURN RANDOM_CHOICE(["打脸爽", "复仇爽"])
    ELSE IF scene_info.has_breakthrough:
        RETURN "升级爽"
    ELSE IF scene_info.has_crisis:
        RETURN "反杀爽"
    ELSE:
        RETURN "装逼爽"
    END IF
END FUNCTION

FUNCTION GENERATE_COOL_POINT(cool_type, parsed_data):
    """生成爽点文本"""
    cool_config = COOL_POINT_TYPES[cool_type]
    structure = cool_config.结构
    
    # 根据结构拆解成3个阶段
    phases = SPLIT(structure, " → ")
    
    cool_text = ""
    FOR phase IN phases:
        cool_text += WRITE_COOL_PHASE(phase, parsed_data) + "\n\n"
    END FOR
    
    RETURN cool_text
END FUNCTION

FUNCTION DECOMPOSE_SCENE_TO_UNITS(scene_idx, parsed_data):
    """
    将场景拆解为写作单元
    从Capsule推断场景应该包含什么
    """
    units = []
    
    # 基础单元：开场环境（重要度5）
    units.APPEND({
        "type": "描写",
        "importance": 5,
        "content": GET_SCENE_ENVIRONMENT(scene_idx, parsed_data)
    })
    
    # 从§3核心任务推断需要的单元
    mission = parsed_data.goals.core_mission
    mission_units = INFER_UNITS_FROM_MISSION(mission, scene_idx)
    units.EXTEND(mission_units)
    
    # 从§5情绪数据推断情绪单元
    IF scene_idx == 1:  # 开篇场景
        units.APPEND({
            "type": "情绪",
            "importance": 7,
            "emotion_type": parsed_data.emotions.start_emotion,
            "intensity": 60
        })
    END IF
    
    RETURN units
END FUNCTION



FUNCTION COUNT_CLAUDE_DISEASES_V3(text):
    """
    检测Claude病症（完整版：20条）
    """
    disease_count = 0
    
    # ========== 前10条（v3.0已有）==========
    
    # D12: 口水词
    filler_words = ["似乎", "仿佛", "或许", "可能", "大概", "好像"]
    FOR word IN filler_words:
        disease_count += COUNT_OCCURRENCES(text, word)
    END FOR
    
    # D13: 套路转折
    cliche_words = ["然而", "但是", "就在这时", "突然", "忽然"]
    FOR word IN cliche_words:
        disease_count += COUNT_OCCURRENCES(text, word)
    END FOR
    
    # D18: 情绪词直写
    emotion_words = ["震惊", "恐惧", "愤怒", "焦虑"]
    FOR word IN emotion_words:
        disease_count += COUNT_OCCURRENCES(text, word)
    END FOR
    
    # D15: 结尾总结句
    paragraphs = SPLIT_PARAGRAPHS(text)
    FOR para IN paragraphs:
        last_sent = GET_LAST_SENTENCE(para)
        IF CONTAINS_ANY(last_sent, ["他明白了", "他决定了", "他意识到"]):
            disease_count += 1
        END IF
    END FOR
    
    # D19: 动作链
    action_chains = DETECT_ACTION_CHAINS(text)
    disease_count += COUNT(chain FOR chain IN action_chains IF LENGTH(chain) > 4)
    
    # D1: 过度展开（检测单个信息点展开>150字）
    disease_count += COUNT_OVER_EXPANDED_INFO(text, threshold=150)
    
    # D2: 过度解释（检测"这是因为""之所以"等）
    explain_patterns = ["这是因为", "之所以", "原因在于"]
    FOR pattern IN explain_patterns:
        disease_count += COUNT_OCCURRENCES(text, pattern)
    END FOR
    
    # D3: 过度内心戏（检测内心戏占比>15%）
    inner_ratio = CALCULATE_INNER_MONOLOGUE_RATIO(text)
    IF inner_ratio > 0.15:
        disease_count += ROUND((inner_ratio - 0.15) * 100)  # 每超1%加1分
    END IF
    
    # D16: 不敢留白（检测过度暗示）
    hint_patterns = ["似乎预示着", "可能会", "或许意味着"]
    FOR pattern IN hint_patterns:
        disease_count += COUNT_OCCURRENCES(text, pattern)
    END FOR
    
    # D20: 视角漂移（检测"XX想""XX觉得"出现在非主角身上）
    disease_count += COUNT_POV_SHIFTS(text, parsed_data.characters.protagonist.name)
    
    # ========== 后10条（新增检测）==========
    
    # D4: 过度安全
    hedging = ["可以说", "基本上", "在某种意义上", "某种程度上"]
    FOR phrase IN hedging:
        disease_count += COUNT_OCCURRENCES(text, phrase)
    END FOR
    
    # D5: 过度冗余
    disease_count += DETECT_REPETITION_COUNT(text)
    
    # D14: 过度铺垫
    disease_count += DETECT_OVER_SETUP_COUNT(text)
    
    # D17: 不敢跳跃
    transition_phrases = ["过了", "来到", "走到", "片刻之后"]
    FOR phrase IN transition_phrases:
        disease_count += COUNT_OCCURRENCES(text, phrase)
    END FOR
    
    # D7: 过度逻辑
    causality = ["因为", "所以", "由于", "导致"]
    FOR word IN causality:
        disease_count += COUNT_OCCURRENCES(text, word) * 0.5  # 因果词适当打折
    END FOR
    
    # D8: 过度完整
    disease_count += COUNT_COMPLETE_STRUCTURES(text)
    
    # D11: 信息密度不足
    low_density_paras = FIND_LOW_DENSITY_PARAGRAPHS(text, threshold=0.005)
    disease_count += LENGTH(low_density_paras)
    
    # D6, D9, D10: 在写作阶段已通过拟人化任务处理，不重复检测
    
    RETURN disease_count
END FUNCTION

# ==================== 新增检测辅助函数 ====================

FUNCTION COUNT_OVER_EXPANDED_INFO(text, threshold):
    """检测过度展开的信息点"""
    info_points = DETECT_INFO_POINTS(text)
    over_expanded = 0
    
    FOR point IN info_points:
        IF point.word_count > threshold:
            over_expanded += 1
        END IF
    END FOR
    
    RETURN over_expanded
END FUNCTION

FUNCTION COUNT_POV_SHIFTS(text, protagonist_name):
    """检测视角漂移"""
    shift_count = 0
    thought_patterns = [r"(\w+)(想|觉得|认为|心想)", r"(\w+)的(内心|心里)"]
    
    FOR pattern IN thought_patterns:
        matches = FIND_ALL(text, pattern)
        FOR match IN matches:
            character_name = EXTRACT_CHARACTER(match)
            IF character_name != protagonist_name:
                shift_count += 1
            END IF
        END FOR
    END FOR
    
    RETURN shift_count
END FUNCTION

FUNCTION DETECT_REPETITION_COUNT(text):
    """检测重复表达的次数"""
    paragraphs = SPLIT_PARAGRAPHS(text)
    repetition_count = 0
    
    FOR para IN paragraphs:
        sentences = SPLIT_SENTENCES(para)
        FOR i = 0 TO LENGTH(sentences) - 3:
            IF ARE_SEMANTICALLY_SIMILAR(sentences[i], sentences[i+1], sentences[i+2]):
                repetition_count += 1
            END IF
        END FOR
    END FOR
    
    RETURN repetition_count
END FUNCTION

FUNCTION DETECT_OVER_SETUP_COUNT(text):
    """检测过度铺垫的次数"""
    paragraphs = SPLIT_PARAGRAPHS(text)
    over_setup_count = 0
    
    FOR i = 0 TO LENGTH(paragraphs) - 2:
        IF LENGTH(paragraphs[i]) > 100 AND CONTAINS_ACTION_TRIGGER(paragraphs[i+1]):
            over_setup_count += 1
        END IF
    END FOR
    
    RETURN over_setup_count
END FUNCTION

FUNCTION COUNT_COMPLETE_STRUCTURES(text):
    """检测完整的起承转合结构"""
    scenes = DETECT_SCENES(text)
    complete_count = 0
    
    FOR scene IN scenes:
        structure = ANALYZE_SCENE_STRUCTURE(scene)
        IF structure.has_all_four_parts:
            complete_count += 1
        END IF
    END FOR
    
    RETURN complete_count
END FUNCTION

FUNCTION FIND_LOW_DENSITY_PARAGRAPHS(text, threshold):
    """找出低密度段落"""
    paragraphs = SPLIT_PARAGRAPHS(text)
    low_density = []
    
    FOR para IN paragraphs:
        IF LENGTH(para) > 100:  # 只检查长段落
            density = CALCULATE_INFO_DENSITY(para)
            IF density < threshold:
                low_density.APPEND(para)
            END IF
        END IF
    END FOR
    
    RETURN low_density
END FUNCTION

```

---

## 【执行示例】

```python
# ==================== 示例执行流程 ====================

# 1. 读取胶囊
CAPSULE = READ_FILE("Capsule.md")

# 2. 执行主流程
PRINT "==================== 开始执行 ===================="
output = MAIN_EXECUTION(CAPSULE)

# 3. 输出结果

# 3.1 快速摘要
PRINT output.summary

# 3.2 章节正文
PRINT "\n==================== 章节正文 ====================\n"
PRINT output.chapter_content

# 3.3 新增Fact
PRINT "\n==================== 新增Fact ====================\n"
PRINT output.new_facts

# 3.4 详细诊断（可选）
IF EXISTS(output, "details"):
    PRINT "\n"
    PRINT output.details
END IF

PRINT "\n==================== 执行完成 ===================="
```

---


### 📋 执行要点

1. **严格执行任务清单**：Claude必须完成humanizer.task_checklist中的所有MUST任务

2. **字数预算管理**：每个场景有明确字数预算，不超标

3. **集中润色**：写作时不必担心病症，润色阶段统一处理

4. **实时检查**：仅检查红线违反和严重超标，其他问题留给全局诊断

5. **问题分析**：重写前必须先分析问题根源，避免盲目重写

### ⚠️ 注意事项

1. **Capsule优先**：如果Capsule中明确指定了某些内容（如爽点、伏笔显眼度），直接使用，不自动生成

2. **重写次数限制**：最多重写2次，超过则强制交付并标记问题

3. **紧急干预门槛**：只有CRITICAL问题才触发场景重写，WARNING仅记录

4. **输出分层**：快速摘要→问题清单→正文→详细数据，用户可按需查看

---

**END OF SOP v3.0**

---
