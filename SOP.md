# 📋 Claude写作执行SOP v3.1 

---

## 【模块0】GLOBAL_CONFIG - 全局配置

```python
# ==================== 全局常量 ====================
CONST WORD_COUNT_TARGET = 4000  # 目标字数
CONST WORD_COUNT_MIN = 3000     # 最小字数（硬约束）
CONST WORD_COUNT_MAX = 6000     # 最大字数（硬约束）
CONST MAX_REWRITE_ATTEMPTS = 2  # 最大重写次数

CONST SCENE_WORD_MIN = 300      # 单场景最小字数
CONST SCENE_WORD_MAX = 1200     # 单场景最大字数
CONST SCENE_WORD_AVG = 800      # 单场景平均字数

# ==================== 番茄风格核心规则 ====================
CONST TOMATO_CORE_RULES = {
    "pace": "快节奏，不拖泥带水",
    "hook_frequency": 600,           # 每600字一个小钩子
    "dialogue_ratio": [0.35, 0.45],  # 对话占比35%-45%
    "inner_monologue_max": 0.15,     # 内心戏不超过15%
    "paragraph_max_length": 150,     # 单段不超过150字
    "info_density_min": 0.01,        # 每100字至少1个信息点
    "conflict_intensity_floor": 30,  # 冲突强度底线
    "conflict_interval_max": 600,    # 冲突最大间隔600字
    "boredom_threshold": 60          # 无聊度阈值
}

# ==================== Claude病症完整清单（20条）====================
CONST CLAUDE_ALL20_DISEASES = {
    "D1_过度展开": "每个信息点都展开200字",
    "D2_过度解释": "每个逻辑都要给读者解释清楚",
    "D3_过度内心戏": "主角一个小动作配500字心理活动",
    "D4_过度安全": "遣词造句谨慎得像在写法律文书",
    "D5_过度冗余": "同一个意思用3种方式重复表达",
    "D6_过度平均": "每个场景、每个角色的笔墨分配都很均匀",
    "D7_过度逻辑": "所有行为都要有明确的因果链",
    "D8_过度完整": "每个场景都要有'起承转合'",
    "D9_过度礼貌": "角色说话都很客气，没有粗鲁/暴躁/省略",
    "D10_过度对称": "主角做A，配角就会回应B，节奏呆板",
    "D11_信息密度不足": "用200字讲一个只需要20字的事",
    "D12_口水词滥用": "大量的'似乎''仿佛''或许''可能'等模糊词",
    "D13_套路化转折": "总是用'然而''但是''就在这时'等老套转折词",
    "D14_过度铺垫": "一个小事件前面铺垫300字",
    "D15_结尾总结癖": "每个场景结尾都要来一句总结性的话",
    "D16_不敢留白": "所有伏笔都要暗示，生怕读者看不懂",
    "D17_不敢跳跃": "时间/空间切换都要详细交代过渡",
    "D18_情绪词滥用": "动不动就'震惊''恐惧''愤怒'等强情绪词",
    "D19_动作链完整癖": "'他站起来，走到门口，伸手推开门，迈步走出去'",
    "D20_视角漂移": "明明是主角视角，却突然知道了配角的想法"
}

# ==================== 病症检测权重 ====================
CONST DISEASE_WEIGHTS = {
    "D1_过度展开": 1.0,
    "D2_过度解释": 1.0,
    "D3_过度内心戏": 1.5,      # 严重违反，加权
    "D4_过度安全": 0.8,
    "D5_过度冗余": 1.2,
    "D6_过度平均": 0.0,         # 写作阶段处理，不计入
    "D7_过度逻辑": 0.5,         # 因果词高频，减权
    "D8_过度完整": 1.0,
    "D9_过度礼貌": 0.0,         # 写作阶段处理，不计入
    "D10_过度对称": 0.0,        # 写作阶段处理，不计入
    "D11_信息密度不足": 1.5,    # 严重违反，加权
    "D12_口水词滥用": 1.0,
    "D13_套路化转折": 1.0,
    "D14_过度铺垫": 1.2,
    "D15_结尾总结癖": 1.0,
    "D16_不敢留白": 0.8,
    "D17_不敢跳跃": 0.8,
    "D18_情绪词滥用": 1.5,      # 严重违反Show原则，加权
    "D19_动作链完整癖": 1.0,
    "D20_视角漂移": 1.5         # 严重错误，加权
}

# ==================== 常用爽点类型（精简到5种）====================
CONST COOL_POINT_TYPES = {
    "打脸爽": {
        "结构": "被质疑 → 瞬间反转 → 对方震惊",
        "强度": 80,
        "使用场景": ["展示实力", "拆穿伪装", "反驳质疑"],
        "插入位置": "冲突后"
    },
    "装逼爽": {
        "结构": "低调隐藏实力 → 关键时刻展现 → 众人震撼",
        "强度": 85,
        "使用场景": ["危机时刻", "被轻视后", "保护他人"],
        "插入位置": "场景高潮"
    },
    "复仇爽": {
        "结构": "被欺负 → 记下仇恨 → 当场或延后报复",
        "强度": 90,
        "使用场景": ["被羞辱", "被陷害", "被背叛"],
        "插入位置": "冲突后"
    },
    "升级爽": {
        "结构": "突破境界 → 实力暴涨 → 立刻验证",
        "强度": 95,
        "使用场景": ["修炼", "顿悟", "获得传承"],
        "插入位置": "场景中段"
    },
    "反杀爽": {
        "结构": "濒死绝境 → 突然翻盘 → 反杀敌人",
        "强度": 100,
        "使用场景": ["生死危机", "绝境", "背水一战"],
        "插入位置": "场景末尾"
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

# ==================== 语法规则（统一标准）====================
CONST SYNTAX_RULES = {
    "变量存在检查": "使用 'key' IN dict",
    "字符串包含检查": "使用 keyword IN text",
    "默认值获取": "使用 dict.GET(key, default)",
    "数组过滤": "使用 FILTER(array, condition)",
    "数组映射": "使用 MAP(array, function)"
}
```

---

## 【模块1】MAIN_EXECUTION_LOOP - 主执行流程

```python
FUNCTION MAIN_EXECUTION(CAPSULE, existing_humanizer=NULL):
    """
    主执行流程：集成实时轻量检查 + 最终全局诊断
    
    参数:
        CAPSULE: 信息胶囊内容
        existing_humanizer: 重写时传入的humanizer（保留状态）
    """
    
    # ========== STEP 1: 解析胶囊 ==========
    PRINT "[SYSTEM] 开始解析信息胶囊..."
    TRY:
        parsed_data = PARSE_CAPSULE(CAPSULE)
        VALIDATE_CAPSULE_INTEGRITY(parsed_data)
    CATCH CapsuleParseError AS e:
        PRINT "[ERROR] 胶囊解析失败: {e.message}"
        RETURN ERROR_REPORT(e)
    END TRY
    
    # ========== STEP 2: 生成或恢复任务清单 ==========
    IF existing_humanizer IS NULL:
        PRINT "[SYSTEM] 生成拟人化任务清单..."
        humanizer = INIT_HUMANIZE_ENGINE_V3(parsed_data)
        humanizer.rewrite_count = 0
    ELSE:
        PRINT "[SYSTEM] 恢复humanizer状态（第{existing_humanizer.rewrite_count}次重写）..."
        humanizer = existing_humanizer
    END IF
    
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
        
        # 4.1 检查是否需要强制收尾
        IF monitors.word_count > WORD_COUNT_MAX * 0.85:
            PRINT "[WARNING] 字数接近上限({monitors.word_count}/{WORD_COUNT_MAX})，准备收尾..."
            humanizer.force_ending = TRUE
            # 跳过剩余场景，直接写结尾
            IF scene_idx < scene_count:
                scene_count = scene_idx  # 截断场景数
            END IF
        END IF
        
        # 4.2 写作场景
        TRY:
            scene_text = WRITE_SCENE_V3(
                scene_idx, 
                parsed_data, 
                humanizer, 
                chapter_content
            )
        CATCH SceneWriteError AS e:
            PRINT "[ERROR] 场景{scene_idx}写作失败: {e.message}"
            scene_text = WRITE_FALLBACK_SCENE(scene_idx, parsed_data)
        END TRY
        
        chapter_content += scene_text
        monitors.word_count += LENGTH(scene_text)
        
        # 4.3 实时轻量检查
        check_result = LIGHTWEIGHT_SCENE_CHECK(
            scene_text, 
            scene_idx, 
            parsed_data, 
            monitors
        )
        
        monitors.scene_checks.APPEND(check_result)
        
        # 4.4 紧急干预（仅处理CRITICAL问题）
        IF check_result.severity == "CRITICAL":
            PRINT "[EMERGENCY] 场景{scene_idx}发现严重问题，立即重写..."
            
            TRY:
                scene_text = REWRITE_SCENE_WITH_FIX(
                    scene_idx, 
                    parsed_data, 
                    humanizer, 
                    check_result.issue
                )
                chapter_content = REPLACE_LAST_SCENE(chapter_content, scene_text)
                monitors.word_count = LENGTH(chapter_content)  # 重新计算
            CATCH RewriteError AS e:
                PRINT "[ERROR] 重写失败，保留原场景: {e.message}"
            END TRY
        END IF
    END FOR
    
    # ========== STEP 5: 集中反Claude病润色 ==========
    PRINT "[SYSTEM] 执行集中反Claude病润色..."
    TRY:
        chapter_content = APPLY_ALL_ANTI_DISEASE_RULES(chapter_content, parsed_data)
    CATCH PolishError AS e:
        PRINT "[WARNING] 润色部分失败: {e.message}"
        # 继续执行，不中断
    END TRY
    
    # ========== STEP 6: 全局诊断 ==========
    PRINT "[SYSTEM] 开始全局诊断..."
    diagnosis = DIAGNOSE_CHAPTER_V3(chapter_content, parsed_data, monitors)
    
    # ========== STEP 7: 修正或重写 ==========
    IF diagnosis.has_critical_issues:
        IF humanizer.rewrite_count < MAX_REWRITE_ATTEMPTS:
            # 问题分析
            PRINT "[SYSTEM] 分析问题原因..."
            problem_analysis = ANALYZE_WHAT_WENT_WRONG(diagnosis, parsed_data, chapter_content)
            
            PRINT "[SYSTEM] 生成修正指令..."
            fix_instruction = GENERATE_FIX_INSTRUCTION(problem_analysis)
            
            humanizer.rewrite_count += 1
            humanizer.fix_instruction = fix_instruction
            
            PRINT "[SYSTEM] 第{humanizer.rewrite_count}次重写（针对性修正）..."
            
            # 递归重写（传递humanizer状态）
            RETURN MAIN_EXECUTION(CAPSULE, humanizer)
        ELSE:
            PRINT "[ERROR] 已达最大重写次数({MAX_REWRITE_ATTEMPTS})，强制交付"
            diagnosis.forced_delivery = TRUE
        END IF
    END IF
    
    # ========== STEP 8: 提取Fact ==========
    PRINT "[SYSTEM] 提取新增Fact..."
    new_facts = EXTRACT_FACTS(chapter_content, parsed_data)
    
    # ========== STEP 9: 交付 ==========
    PRINT "[SYSTEM] 生成交付输出..."
    RETURN DELIVER_OUTPUT_V3(chapter_content, new_facts, diagnosis, monitors)
END FUNCTION
```

---

## 【模块2】HUMANIZE_ENGINE_V3 - 拟人化引擎

```python
FUNCTION INIT_HUMANIZE_ENGINE_V3(parsed_data):
    """
    拟人化引擎v3：从概率机制改为任务清单机制
    核心改进：明确告诉Claude"哪个场景必须做什么"
    """
    
    humanizer = {
        "task_checklist": {},    # {scene_idx: [task1, task2, ...]}
        "scene_budget": {},      # {scene_idx: word_count}
        "cool_point_plan": {},   # {scene_idx: cool_type}
        "foreshadow_plan": {},   # {scene_idx: {content, visibility}}
        "rewrite_count": 0,
        "force_ending": FALSE,
        "fix_instruction": NULL  # 重写时的修正指令
    }
    
    # ========== 估算场景数量 ==========
    scene_count = ESTIMATE_SCENE_COUNT(parsed_data)
    PRINT "[HUMANIZE] 估算场景数量: {scene_count}"
    
    # ========== 生成任务清单 ==========
    FOR scene_idx = 1 TO scene_count:
        tasks = []
        
        # 任务1：开篇方式（仅第一场景）
        IF scene_idx == 1:
            opening_style = RANDOM_CHOICE([
                "动作直入",   # 直接动作，无铺垫
                "对话直入",   # 直接对话，无背景
                "感官直入",   # 感官细节，快速带入
                "冲突直入"    # 直接冲突，不解释原因
            ])
            tasks.APPEND({
                "type": "开篇",
                "action": opening_style,
                "priority": "MUST",
                "description": f"使用{opening_style}，不超过3句话进入场景"
            })
        END IF
        
        # 任务2：跳跃式切入（30%场景）
        IF scene_idx > 1 AND RANDOM() < 0.3:
            tasks.APPEND({
                "type": "跳跃切入",
                "action": "删除场景前3句铺垫，直接从动作/对话开始",
                "priority": "MUST",
                "description": "不写过渡句，直接进入关键动作"
            })
        END IF
        
        # 任务3：故意省略（选择2-3个场景）
        # 修复：明确分配到具体场景
        IF scene_idx IN RANDOM_SAMPLE(RANGE(2, scene_count+1), MIN(2, scene_count-1)):
            tasks.APPEND({
                "type": "故意省略",
                "action": "省略1个动机解释",
                "priority": "SHOULD",
                "description": "找到包含'因为/为了/想要'的句子，删除1句",
                "target_keywords": ["因为", "为了", "想要", "之所以"]
            })
        END IF
        
        # 任务4：笔墨失衡（选择1-2个场景）
        IF scene_idx IN RANDOM_SAMPLE(RANGE(2, scene_count+1), MIN(2, scene_count-1)):
            tasks.APPEND({
                "type": "笔墨失衡",
                "action": "某个次要元素突然多写50字",
                "priority": "SHOULD",
                "description": "选择出现次数<=2的物品或配角，扩展描写",
                "target_length": 50
            })
        END IF
        
        # 任务5：对话不回应（25%场景）
        IF RANDOM() < 0.25:
            tasks.APPEND({
                "type": "对话不回应",
                "action": "至少有1处：A说话，B不接茬，直接做别的",
                "priority": "SHOULD",
                "description": "对话中插入1次不对称回应"
            })
        END IF
        
        humanizer.task_checklist[scene_idx] = tasks
    END FOR
    
    # ========== 分配场景字数预算 ==========
    total_budget = parsed_data.meta.GET("word_count_target", WORD_COUNT_TARGET)
    scene_importances = GET_SCENE_IMPORTANCES(parsed_data, scene_count)
    importance_sum = SUM(scene_importances.VALUES())
    
    FOR scene_idx = 1 TO scene_count:
        importance = scene_importances.GET(scene_idx, 5)  # 默认重要性5
        
        # 按重要性分配预算
        budget = total_budget * (importance / importance_sum)
        
        # 约束在合理范围
        budget = CLAMP(budget, SCENE_WORD_MIN, SCENE_WORD_MAX)
        
        humanizer.scene_budget[scene_idx] = budget
        PRINT "[HUMANIZE] 场景{scene_idx}预算: {budget:.0f}字（重要性{importance}）"
    END FOR
    
    # ========== 爽点计划 ==========
    IF "cool_point_plan" IN parsed_data.goals:
        # 从Capsule直接读取
        humanizer.cool_point_plan = parsed_data.goals.cool_point_plan
        PRINT "[HUMANIZE] 使用Capsule指定的爽点计划"
    ELSE:
        # 自动规划：平均每1500字一个爽点
        cool_point_count = MAX(1, ROUND(total_budget / 1500))
        cool_point_scenes = DISTRIBUTE_EVENLY(RANGE(1, scene_count+1), cool_point_count)
        
        FOR scene_idx IN cool_point_scenes:
            suitable_type = SELECT_COOL_POINT_TYPE_FOR_SCENE(scene_idx, parsed_data, scene_count)
            humanizer.cool_point_plan[scene_idx] = suitable_type
            PRINT "[HUMANIZE] 场景{scene_idx}计划爽点: {suitable_type}"
        END FOR
    END IF
    
    # ========== 伏笔计划 ==========
    IF "foreshadowing" IN parsed_data.goals:
        FOR foreshadow IN parsed_data.goals.foreshadowing:
            scene_idx = CHOOSE_SCENE_FOR_FORESHADOW(foreshadow, scene_count, parsed_data)
            
            humanizer.foreshadow_plan[scene_idx] = {
                "content": foreshadow.GET("content", ""),
                "visibility": foreshadow.GET("visibility", "隐蔽")  # 默认隐蔽
            }
            PRINT "[HUMANIZE] 场景{scene_idx}计划伏笔: {foreshadow.content[:20]}..."
        END FOR
    END IF
    
    PRINT "[HUMANIZE] 任务清单生成完成"
    
    RETURN humanizer
END FUNCTION
```

---

## 【模块3】LIGHTWEIGHT_SCENE_CHECK - 实时轻量检查

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
    IF "redlines" IN parsed_data.constraints:
        FOR redline IN parsed_data.constraints.redlines:
            IF CHECK_REDLINE_VIOLATION(scene_text, redline):
                check_result.severity = "CRITICAL"
                check_result.issue = f"违反红线：{redline}"
                RETURN check_result
            END IF
        END FOR
    END IF
    
    # ========== 检查2：字数严重超标（CRITICAL）==========
    IF LENGTH(scene_text) > SCENE_WORD_MAX:
        check_result.severity = "CRITICAL"
        check_result.issue = f"场景{scene_idx}字数过多({LENGTH(scene_text)}字，上限{SCENE_WORD_MAX})，会导致全章超标"
        RETURN check_result
    END IF
    
    # ========== 检查3：字数严重不足（WARNING）==========
    IF LENGTH(scene_text) < SCENE_WORD_MIN:
        check_result.severity = "WARNING"
        check_result.issue = f"场景{scene_idx}字数过少({LENGTH(scene_text)}字，下限{SCENE_WORD_MIN})"
        # WARNING不触发重写，仅记录
    END IF
    
    # ========== 检查4：信息密度过低（WARNING）==========
    info_density = COUNT_NEW_INFO(scene_text) / MAX(LENGTH(scene_text), 1)
    IF info_density < TOMATO_CORE_RULES.info_density_min:
        check_result.severity = "WARNING"
        check_result.issue = f"场景{scene_idx}信息密度过低({info_density:.3f}，最低{TOMATO_CORE_RULES.info_density_min})"
    END IF
    
    # ========== 检查5：冲突缺失（WARNING）==========
    words_since_conflict = monitors.word_count - monitors.last_conflict
    IF words_since_conflict > TOMATO_CORE_RULES.conflict_interval_max:
        check_result.severity = "WARNING"
        check_result.issue = f"已连续{words_since_conflict}字无冲突（上限{TOMATO_CORE_RULES.conflict_interval_max}）"
        
        # 检查当前场景是否有冲突
        IF HAS_CONFLICT(scene_text):
            monitors.last_conflict = monitors.word_count  # 更新冲突位置
        END IF
    END IF
    
    # ========== 检查6：对话占比异常（WARNING）==========
    dialogue_ratio = CALCULATE_DIALOGUE_RATIO(scene_text)
    target_range = TOMATO_CORE_RULES.dialogue_ratio
    
    IF dialogue_ratio < target_range[0] * 0.5 OR dialogue_ratio > target_range[1] * 1.5:
        check_result.severity = "WARNING"
        check_result.issue = f"场景{scene_idx}对话占比异常({dialogue_ratio*100:.1f}%，目标{target_range[0]*100}-{target_range[1]*100}%)"
    END IF
    
    RETURN check_result
END FUNCTION
```

---

## 【模块4】WRITE_SCENE_V3 - 场景写作器

```python
FUNCTION WRITE_SCENE_V3(scene_idx, parsed_data, humanizer, previous_content):
    """
    写作单个场景（v3修复版）
    核心改进：执行任务清单，而非概率决策
    """
    
    scene_text = ""
    scene_budget = humanizer.scene_budget.GET(scene_idx, SCENE_WORD_AVG)
    tasks = humanizer.task_checklist.GET(scene_idx, [])
    
    PRINT "[SCENE] 场景{scene_idx}开始，预算{scene_budget:.0f}字，任务{LENGTH(tasks)}项"
    
    # ========== STEP 1: 执行开篇任务 ==========
    opening_task = FIND_TASK_BY_TYPE(tasks, "开篇")
    
    IF opening_task IS NOT NULL:
        scene_text = EXECUTE_OPENING_TASK(opening_task, parsed_data, scene_idx)
    ELSE:
        scene_text = WRITE_DEFAULT_OPENING(scene_idx, parsed_data, previous_content)
    END IF
    
    # ========== STEP 2: 检查是否需要应用修正指令 ==========
    IF humanizer.fix_instruction IS NOT NULL:
        IF scene_idx IN humanizer.fix_instruction.GET("focus_scenes", []):
            PRINT "[FIX] 场景{scene_idx}应用修正指令"
            scene_text = APPLY_FIX_TO_OPENING(scene_text, humanizer.fix_instruction)
        END IF
    END IF
    
    # ========== STEP 3: 写作场景主体 ==========
    units = DECOMPOSE_SCENE_TO_UNITS(scene_idx, parsed_data, humanizer)
    
    FOR unit IN units:
        # 3.1 字数控制（硬停止）
        IF LENGTH(scene_text) > scene_budget * 1.1:
            PRINT "[BUDGET] 场景{scene_idx}达到预算上限({LENGTH(scene_text)}字)，强制结束"
            BREAK
        END IF
        
        # 3.2 强制收尾检查
        IF humanizer.force_ending AND LENGTH(scene_text) > scene_budget * 0.8:
            PRINT "[ENDING] 触发强制收尾，跳过剩余单元"
            BREAK
        END IF
        
        # 3.3 根据重要性决定展开程度
        expansion_level = DECIDE_EXPANSION_BY_IMPORTANCE(unit.GET("importance", 5))
        
        # 3.4 写作单元
		unit_text = WRITE_UNIT_BY_TYPE(
            unit, 
            expansion_level, 
            parsed_data
        )
        
        scene_text += unit_text
    END FOR
    
    # ========== STEP 4: 执行拟人化任务 ==========
    FOR task IN tasks:
        IF task.type != "开篇":  # 开篇任务已执行
            scene_text = EXECUTE_HUMANIZE_TASK(scene_text, task, parsed_data)
        END IF
    END FOR
    
    # ========== STEP 5: 注入爽点（如果计划中有）==========
    IF scene_idx IN humanizer.cool_point_plan:
        cool_type = humanizer.cool_point_plan[scene_idx]
        PRINT "[COOL] 场景{scene_idx}注入爽点: {cool_type}"
        
        cool_text = GENERATE_COOL_POINT(cool_type, parsed_data, scene_idx)
        scene_text = INSERT_COOL_POINT(scene_text, cool_text, cool_type)
    END IF
    
    # ========== STEP 6: 埋伏笔（如果计划中有）==========
    IF scene_idx IN humanizer.foreshadow_plan:
        foreshadow = humanizer.foreshadow_plan[scene_idx]
        PRINT "[FORESHADOW] 场景{scene_idx}埋伏笔: {foreshadow.content[:20]}..."
        
        foreshadow_text = WRITE_FORESHADOW(foreshadow, parsed_data)
        scene_text = INSERT_FORESHADOW(scene_text, foreshadow_text, foreshadow.visibility)
    END IF
    
    # ========== STEP 7: 场景结尾 ==========
    ending = WRITE_SCENE_ENDING(scene_idx, parsed_data, humanizer, scene_text)
    scene_text += ending
    
    PRINT "[SCENE] 场景{scene_idx}完成，实际{LENGTH(scene_text)}字"
    
    RETURN scene_text
END FUNCTION

# ==================== 辅助函数 ====================

FUNCTION DECIDE_EXPANSION_BY_IMPORTANCE(importance):
    """根据重要性决定展开程度"""
    IF importance >= 8:
        RETURN "EXPAND"      # 展开50-100字
    ELSE IF importance >= 5:
        RETURN "BRIEF"       # 简写20-50字
    ELSE:
        RETURN "SKIP"        # 一句话带过或跳过
    END IF
END FUNCTION

FUNCTION WRITE_UNIT_BY_TYPE(unit, expansion_level, parsed_data):
    """根据单元类型写作"""
    unit_type = unit.GET("type", "描写")
    
    SWITCH unit_type:
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
        
        DEFAULT:
            RETURN ""
    END SWITCH
END FUNCTION

FUNCTION EXECUTE_HUMANIZE_TASK(scene_text, task, parsed_data):
    """执行拟人化任务（修复版：边界条件处理）"""
    
    SWITCH task.type:
        CASE "跳跃切入":
            RETURN REMOVE_FIRST_N_SENTENCES_SAFE(scene_text, 3, parsed_data)
        
        CASE "故意省略":
            RETURN REMOVE_ONE_MOTIVATION(scene_text, task.GET("target_keywords", []))
        
        CASE "笔墨失衡":
            RETURN EXPAND_MINOR_ELEMENT(scene_text, task.GET("target_length", 50), parsed_data)
        
        CASE "对话不回应":
            # 已在对话写作时处理
            RETURN scene_text
        
        DEFAULT:
            RETURN scene_text
    END SWITCH
END FUNCTION

FUNCTION REMOVE_FIRST_N_SENTENCES_SAFE(scene_text, n, parsed_data):
    """
    安全地删除前N句（修复版：检查关键信息）
    """
    sentences = SPLIT_SENTENCES(scene_text)
    
    IF LENGTH(sentences) <= n:
        PRINT "[SKIP] 场景句子数不足{n}句，跳过跳跃切入"
        RETURN scene_text
    END IF
    
    first_n = sentences[0:n]
    first_n_text = JOIN(first_n, "")
    
    # 检查是否包含关键信息
    has_critical = (
        CONTAINS_REDLINE_KEYWORDS(first_n_text, parsed_data) OR
        CONTAINS_CORE_MISSION_KEYWORDS(first_n_text, parsed_data) OR
        CONTAINS_CHARACTER_INTRO(first_n_text, parsed_data)
    )
    
    IF has_critical:
        PRINT "[SKIP] 前{n}句包含关键信息，跳过跳跃切入"
        RETURN scene_text
    END IF
    
    RETURN JOIN(sentences[n:], "")
END FUNCTION

FUNCTION REMOVE_ONE_MOTIVATION(scene_text, target_keywords):
    """
    删除一个动机解释（修复版：明确定位）
    """
    IF LENGTH(target_keywords) == 0:
        target_keywords = ["因为", "为了", "想要", "之所以"]
    END IF
    
    sentences = SPLIT_SENTENCES(scene_text)
    motivation_indices = []
    
    # 找到所有包含动机关键词的句子
    FOR i, sent IN ENUMERATE(sentences):
        FOR keyword IN target_keywords:
            IF keyword IN sent AND LENGTH(sent) > 10:
                motivation_indices.APPEND(i)
                BREAK
            END IF
        END FOR
    END FOR
    
    IF LENGTH(motivation_indices) == 0:
        PRINT "[SKIP] 未找到动机句，跳过故意省略"
        RETURN scene_text
    END IF
    
    # 随机选择一个删除
    target_idx = RANDOM_CHOICE(motivation_indices)
    sentences[target_idx] = ""  # 删除该句
    
    RETURN JOIN(FILTER(sentences, lambda s: LENGTH(s) > 0), "")
END FUNCTION

FUNCTION EXPAND_MINOR_ELEMENT(scene_text, target_length, parsed_data):
    """
    扩展次要元素描写（修复版：明确识别）
    """
    # 提取所有名词
    nouns = EXTRACT_NOUNS(scene_text)
    
    # 统计出现次数
    noun_counts = COUNT_OCCURRENCES_EACH(nouns)
    
    # 筛选出现次数<=2的次要元素
    minor_elements = FILTER(noun_counts.ITEMS(), lambda item: item[1] <= 2)
    
    IF LENGTH(minor_elements) == 0:
        PRINT "[SKIP] 未找到次要元素，跳过笔墨失衡"
        RETURN scene_text
    END IF
    
    # 随机选择一个
    target_noun, count = RANDOM_CHOICE(minor_elements)
    
    # 生成扩展描写
    expansion = GENERATE_MINOR_ELEMENT_DESCRIPTION(target_noun, target_length, parsed_data)
    
    # 在第一次出现该名词的位置后插入
    first_occurrence_pos = FIND_FIRST(scene_text, target_noun)
    
    IF first_occurrence_pos == -1:
        RETURN scene_text
    END IF
    
    # 找到该句的结束位置
    sentence_end = FIND_NEXT(scene_text, "。", first_occurrence_pos)
    
    IF sentence_end == -1:
        RETURN scene_text
    END IF
    
    # 插入扩展描写
    RETURN scene_text[:sentence_end+1] + expansion + scene_text[sentence_end+1:]
END FUNCTION
```

---

## 【模块5】ANTI_DISEASE_PROTOCOL_V3 - 集中反Claude病

```python
FUNCTION APPLY_ALL_ANTI_DISEASE_RULES(chapter_content, parsed_data):
    """
    集中应用所有反Claude病规则（修复版：加权处理）
    在润色阶段统一处理，而非分散在写作过程中
    """
    
    PRINT "[POLISH] 开始集中反Claude病润色（20条规则）..."
    
    polished = chapter_content
    disease_log = {}  # 记录每种病症的处理次数
    
    # ========== 病症1：删除口水词（D12）==========
    filler_words = ["似乎", "仿佛", "或许", "可能", "大概", "好像", "某种程度上"]
    count = 0
    FOR word IN filler_words:
        occurrences = COUNT_OCCURRENCES(polished, word)
        polished = REPLACE_ALL(polished, word, "")
        count += occurrences
    END FOR
    disease_log["D12_口水词"] = count * DISEASE_WEIGHTS["D12_口水词滥用"]
    
    # ========== 病症2：删除套路转折词（D13）==========
    cliche_words = ["然而", "但是", "就在这时", "突然", "忽然", "霎时间"]
    count = 0
    FOR word IN cliche_words:
        occurrences = COUNT_OCCURRENCES(polished, word)
        polished = REPLACE_ALL(polished, word, "")
        count += occurrences
    END FOR
    disease_log["D13_套路转折"] = count * DISEASE_WEIGHTS["D13_套路化转折"]
    
    # ========== 病症3：替换情绪词为生理反应（D18）==========
    polished, emotion_count = REPLACE_EMOTION_WORDS_WITH_PHYSIOLOGY(polished)
    disease_log["D18_情绪词"] = emotion_count * DISEASE_WEIGHTS["D18_情绪词滥用"]
    
    # ========== 病症4：简化动作链（D19）==========
    polished, action_count = SIMPLIFY_ACTION_CHAINS(polished)
    disease_log["D19_动作链"] = action_count * DISEASE_WEIGHTS["D19_动作链完整癖"]
    
    # ========== 病症5：删除段落总结句（D15）==========
    polished, summary_count = REMOVE_PARAGRAPH_SUMMARIES(polished)
    disease_log["D15_总结癖"] = summary_count * DISEASE_WEIGHTS["D15_结尾总结癖"]
    
    # ========== 病症6：压缩过长段落（通用）==========
    polished, compress_count = COMPRESS_LONG_PARAGRAPHS(polished, TOMATO_CORE_RULES.paragraph_max_length)
    disease_log["段落过长"] = compress_count
    
    # ========== 病症7：减少内心戏（D3）==========
    polished, inner_count = REDUCE_INNER_MONOLOGUE(polished, TOMATO_CORE_RULES.inner_monologue_max)
    disease_log["D3_内心戏"] = inner_count * DISEASE_WEIGHTS["D3_过度内心戏"]
    
    # ========== 病症8：删除冗余（D5）==========
    polished, redundancy_count = REMOVE_REDUNDANCY(polished)
    disease_log["D5_冗余"] = redundancy_count * DISEASE_WEIGHTS["D5_过度冗余"]
    
    # ========== 病症9：优化句长（通用）==========
    polished = OPTIMIZE_SENTENCE_LENGTH(polished)
    
    # ========== 病症10：删除解释性内心戏（D2）==========
    polished, explain_count = REMOVE_EXPLANATORY_THOUGHTS(polished)
    disease_log["D2_解释"] = explain_count * DISEASE_WEIGHTS["D2_过度解释"]
    
    # ========== 病症11：过度安全（D4）==========
    hedging_phrases = ["可以说", "基本上", "在某种意义上", "某种角度", "相对而言", "总的来说"]
    count = 0
    FOR phrase IN hedging_phrases:
        occurrences = COUNT_OCCURRENCES(polished, phrase)
        polished = REPLACE_ALL(polished, phrase, "")
        count += occurrences
    END FOR
    disease_log["D4_安全"] = count * DISEASE_WEIGHTS["D4_过度安全"]
    
    # ========== 病症12：过度铺垫（D14）==========
    polished, setup_count = COMPRESS_SETUP(polished)
    disease_log["D14_铺垫"] = setup_count * DISEASE_WEIGHTS["D14_过度铺垫"]
    
    # ========== 病症13：不敢跳跃（D17）==========
    polished, transition_count = REMOVE_TRANSITIONS(polished)
    disease_log["D17_跳跃"] = transition_count * DISEASE_WEIGHTS["D17_不敢跳跃"]
    
    # ========== 病症14：过度逻辑（D7）==========
    polished, causality_count = REMOVE_CAUSALITY_EXPLANATION(polished)
    disease_log["D7_逻辑"] = causality_count * DISEASE_WEIGHTS["D7_过度逻辑"]
    
    # ========== 病症15：过度完整（D8）==========
    polished, structure_count = BREAK_SCENE_STRUCTURE(polished)
    disease_log["D8_完整"] = structure_count * DISEASE_WEIGHTS["D8_过度完整"]
    
    # ========== 病症16：过度礼貌（D9）==========
    polished, rough_count = ADD_CONVERSATIONAL_ROUGHNESS(polished)
    disease_log["D9_礼貌"] = rough_count  # 权重为0，仅记录
    
    # ========== 病症17：信息密度不足（D11/D20）==========
    polished, density_count = COMPRESS_LOW_DENSITY_PARAGRAPHS(polished)
    disease_log["D11_密度"] = density_count * DISEASE_WEIGHTS["D11_信息密度不足"]
    
    # ========== 病症18：不敢留白（D16）==========
    hint_patterns = ["似乎预示着", "可能会", "或许意味着", "暗示着"]
    count = 0
    FOR pattern IN hint_patterns:
        occurrences = COUNT_OCCURRENCES(polished, pattern)
        polished = REPLACE_ALL(polished, pattern, "")
        count += occurrences
    END FOR
    disease_log["D16_留白"] = count * DISEASE_WEIGHTS["D16_不敢留白"]
    
    # ========== 病症19：视角漂移（D20）==========
    protagonist_name = parsed_data.characters.protagonist.GET("name", "主角")
    polished, pov_count = FIX_POV_SHIFTS(polished, protagonist_name)
    disease_log["D20_视角"] = pov_count * DISEASE_WEIGHTS["D20_视角漂移"]
    
    # ========== 病症20：过度展开（D1）==========
    polished, expand_count = COMPRESS_OVER_EXPANDED_INFO(polished)
    disease_log["D1_展开"] = expand_count * DISEASE_WEIGHTS["D1_过度展开"]
    
    # ========== 输出病症处理报告 ==========
    total_weighted_score = SUM(disease_log.VALUES())
    PRINT "[POLISH] 反Claude病润色完成，加权病症分: {total_weighted_score:.1f}"
    
    FOR disease, score IN disease_log.ITEMS():
        IF score > 0:
            PRINT "[POLISH]   - {disease}: {score:.1f}分"
        END IF
    END FOR
    
    RETURN polished
END FUNCTION

# ==================== 具体实现函数====================

FUNCTION REPLACE_EMOTION_WORDS_WITH_PHYSIOLOGY(text):
    """替换情绪词为生理反应（修复版：返回计数）"""
    emotion_map = {
        "震惊": ["瞳孔骤然收缩", "呼吸一滞", "僵在原地"],
        "恐惧": ["后背发凉", "腿发软", "冷汗浸透后背"],
        "愤怒": ["太阳穴跳动", "拳头攥紧", "脸颊发烫"],
        "焦虑": ["胃部收紧", "手心冒汗", "呼吸变浅"],
        "紧张": ["喉咙发干", "心跳加速", "肌肉紧绷"],
        "兴奋": ["心跳如鼓", "呼吸急促", "眼睛发亮"],
        "悲伤": ["鼻腔发酸", "眼眶发热", "喉头发紧"]
    }
    
    replace_count = 0
    
    FOR emotion_word, reactions IN emotion_map.ITEMS():
        # 匹配模式："他/她 + 感到/充满/满是 + 情绪词"
        patterns = [
            f"(他|她|\\w{{2,4}}).*感到{emotion_word}",
            f"(他|她|\\w{{2,4}}).*充满{emotion_word}",
            f"(他|她|\\w{{2,4}}).*满是{emotion_word}",
            f"{emotion_word}(涌上|涌起|袭来)"
        ]
        
        FOR pattern IN patterns:
            matches = FIND_ALL_WITH_CONTEXT(text, pattern)
            
            FOR match IN matches:
                # 提取主语
                subject = EXTRACT_SUBJECT(match)
                
                # 随机选择生理反应
                reaction = RANDOM_CHOICE(reactions)
                
                # 生成替换文本
                IF subject:
                    replacement = f"{subject}的{reaction}"
                ELSE:
                    replacement = reaction
                END IF
                
                text = REPLACE_FIRST(text, match.full_text, replacement)
                replace_count += 1
            END FOR
        END FOR
    END FOR
    
    RETURN text, replace_count
END FUNCTION

FUNCTION SIMPLIFY_ACTION_CHAINS(text):
    """简化动作链（修复版：返回计数）"""
    # 匹配模式：连续4个以上动作，用"然后/接着/，"连接
    pattern = r"(他|她|\\w{2,4})(\\w{2,5})(，|然后|接着)(\\w{2,5})(，|然后|接着)(\\w{2,5})(，|然后|接着)(\\w{2,5})"
    
    matches = FIND_ALL_WITH_CONTEXT(text, pattern)
    simplify_count = 0
    
    FOR match IN matches:
        actions = EXTRACT_ACTIONS(match)  # 提取所有动作词
        
        IF LENGTH(actions) < 4:
            CONTINUE
        END IF
        
        # 选择关键动作（通常是最后一个）
        key_action = actions[-1]
        
        # 提取主语
        subject = EXTRACT_SUBJECT(match)
        
        # 简化为一个动作
        simplified = f"{subject}{key_action}"
        
        text = REPLACE_FIRST(text, match.full_text, simplified)
        simplify_count += 1
    END FOR
    
    RETURN text, simplify_count
END FUNCTION

FUNCTION REMOVE_PARAGRAPH_SUMMARIES(text):
    """删除段落总结句（修复版：返回计数）"""
    paragraphs = SPLIT_PARAGRAPHS(text)
    remove_count = 0
    
    summary_patterns = [
        "他明白了",
        "他意识到",
        "他决定了",
        "他知道",
        "他想通了",
        "他恍然大悟",
        "原来如此"
    ]
    
    FOR i, para IN ENUMERATE(paragraphs):
        sentences = SPLIT_SENTENCES(para)
        
        IF LENGTH(sentences) == 0:
            CONTINUE
        END IF
        
        last_sent = sentences[-1]
        
        # 检查最后一句是否包含总结模式
        has_summary = FALSE
        FOR pattern IN summary_patterns:
            IF pattern IN last_sent:
                has_summary = TRUE
                BREAK
            END IF
        END FOR
        
        IF has_summary:
            # 删除最后一句
            sentences = sentences[:-1]
            paragraphs[i] = JOIN(sentences, "")
            remove_count += 1
        END IF
    END FOR
    
    RETURN JOIN(FILTER(paragraphs, lambda p: LENGTH(p) > 0), "\n\n"), remove_count
END FUNCTION

FUNCTION COMPRESS_LONG_PARAGRAPHS(text, max_length):
    """压缩过长段落（修复版：返回计数）"""
    paragraphs = SPLIT_PARAGRAPHS(text)
    compress_count = 0
    
    FOR i, para IN ENUMERATE(paragraphs):
        IF LENGTH(para) > max_length:
            # 提取核心信息
            core_info = EXTRACT_CORE_INFO(para)
            
            # 重写为简短版本
            compressed = REWRITE_BRIEFLY(core_info, max_length)
            
            paragraphs[i] = compressed
            compress_count += 1
        END IF
    END FOR
    
    RETURN JOIN(paragraphs, "\n\n"), compress_count
END FUNCTION

FUNCTION REDUCE_INNER_MONOLOGUE(text, max_ratio):
    """减少内心戏（修复版：返回计数）"""
    inner_ratio = CALCULATE_INNER_MONOLOGUE_RATIO(text)
    
    IF inner_ratio <= max_ratio:
        RETURN text, 0
    END IF
    
    # 提取所有内心戏片段
    inner_segments = EXTRACT_INNER_MONOLOGUE(text)
    
    # 计算需要删减的比例
    reduce_ratio = 1 - (max_ratio / inner_ratio)
    
    # 按重要性排序，删除不重要的
    inner_segments_sorted = SORT_BY_IMPORTANCE(inner_segments)
    segments_to_remove = inner_segments_sorted[:ROUND(LENGTH(inner_segments) * reduce_ratio)]
    
    FOR segment IN segments_to_remove:
        text = REPLACE_FIRST(text, segment.full_text, "")
    END FOR
    
    RETURN CLEAN_WHITESPACE(text), LENGTH(segments_to_remove)
END FUNCTION

FUNCTION REMOVE_REDUNDANCY(text):
    """删除同一意思的重复表达（修复版：语义判断）"""
    paragraphs = SPLIT_PARAGRAPHS(text)
    remove_count = 0
    
    FOR i, para IN ENUMERATE(paragraphs):
        sentences = SPLIT_SENTENCES(para)
        
        IF LENGTH(sentences) < 3:
            CONTINUE
        END IF
        
        # 检测相邻3句是否语义相似
        j = 0
        WHILE j <= LENGTH(sentences) - 3:
            IF ARE_SEMANTICALLY_SIMILAR(sentences[j], sentences[j+1], sentences[j+2]):
                # 保留第一句，删除后两句
                sentences = sentences[:j+1] + sentences[j+3:]
                remove_count += 1
            ELSE:
                j += 1
            END IF
        END WHILE
        
        paragraphs[i] = JOIN(sentences, "")
    END FOR
    
    RETURN JOIN(FILTER(paragraphs, lambda p: LENGTH(p) > 0), "\n\n"), remove_count
END FUNCTION

FUNCTION OPTIMIZE_SENTENCE_LENGTH(text):
    """优化句长（长短句交错）"""
    paragraphs = SPLIT_PARAGRAPHS(text)
    
    FOR i, para IN ENUMERATE(paragraphs):
        sentences = SPLIT_SENTENCES(para)
        
        IF LENGTH(sentences) < 5:
            CONTINUE
        END IF
        
        # 检测连续5句是否都是长句（>25字）或都是短句（<10字）
        FOR j = 0 TO LENGTH(sentences) - 5:
            segment = sentences[j:j+5]
            lengths = MAP(segment, LENGTH)
            
            all_long = ALL(length > 25 FOR length IN lengths)
            all_short = ALL(length < 10 FOR length IN lengths)
            
            IF all_long:
                # 将中间一句拆分为两句
                middle_idx = j + 2
                sentences[middle_idx] = SPLIT_SENTENCE_AT_COMMA(sentences[middle_idx])
            ELSE IF all_short:
                # 将中间两句合并
                sentences[j+2] = sentences[j+2] + sentences[j+3]
                sentences = sentences[:j+3] + sentences[j+4:]
            END IF
        END FOR
        
        paragraphs[i] = JOIN(sentences, "")
    END FOR
    
    RETURN JOIN(paragraphs, "\n\n")
END FUNCTION

FUNCTION REMOVE_EXPLANATORY_THOUGHTS(text):
    """删除解释性内心戏（修复版：返回计数）"""
    explanatory_patterns = [
        "他这样做是因为",
        "她之所以",
        "这是为了",
        "目的是",
        "原因在于"
    ]
    
    remove_count = 0
    
    FOR pattern IN explanatory_patterns:
        # 找到包含该模式的句子
        sentences_with_pattern = FIND_SENTENCES_CONTAINING(text, pattern)
        
        FOR sentence IN sentences_with_pattern:
            text = REPLACE_FIRST(text, sentence, "")
            remove_count += 1
        END FOR
    END FOR
    
    RETURN CLEAN_WHITESPACE(text), remove_count
END FUNCTION

FUNCTION COMPRESS_SETUP(text):
    """压缩过度铺垫（修复版：返回计数）"""
    paragraphs = SPLIT_PARAGRAPHS(text)
    compress_count = 0
    
    FOR i = 0 TO LENGTH(paragraphs) - 2:
        current = paragraphs[i]
        next = paragraphs[i+1]
        
        # 检测铺垫模式：当前段落>100字 + 下一段落包含动作触发词
        IF LENGTH(current) > 100 AND CONTAINS_ACTION_TRIGGER(next):
            # 提取核心信息
            core = EXTRACT_CORE_INFO(current)
            
            # 压缩到30-50字
            compressed = REWRITE_BRIEFLY(core, 50)
            
            paragraphs[i] = compressed
            compress_count += 1
        END IF
    END FOR
    
    RETURN JOIN(paragraphs, "\n\n"), compress_count
END FUNCTION

FUNCTION REMOVE_TRANSITIONS(text):
    """删除过渡句（修复版：返回计数）"""
    transition_patterns = [
        r"过了.{1,5}(时间|分钟|小时|天|月)",
        r"(他|她|\\w{2,4}).{0,5}(走到|来到|到了).{2,10}",
        r"片刻之后",
        r"不多时",
        r"须臾",
        r"不一会儿"
    ]
    
    remove_count = 0
    
    FOR pattern IN transition_patterns:
        matches = FIND_ALL_WITH_CONTEXT(text, pattern)
        
        FOR match IN matches:
            # 检查是否是独立的过渡句（前后都有句号或换行）
            IF IS_STANDALONE_SENTENCE(text, match):
                text = REPLACE_FIRST(text, match.full_sentence, "")
                remove_count += 1
            END IF
        END FOR
    END FOR
    
    RETURN CLEAN_WHITESPACE(text), remove_count
END FUNCTION

FUNCTION REMOVE_CAUSALITY_EXPLANATION(text):
    """删除因果解释（修复版：返回计数）"""
    causality_patterns = [
        r"因为.{5,30}所以",
        r"之所以.{5,30}是因为",
        r"这(是|样做)(是)?因为",
        r"正(是)?由于"
    ]
    
    remove_count = 0
    
    FOR pattern IN causality_patterns:
        matches = FIND_ALL_WITH_CONTEXT(text, pattern)
        
        FOR match IN matches:
            # 保留"所以"后面的结果部分，删除"因为"部分
            result_part = EXTRACT_RESULT_CLAUSE(match)
            
            IF result_part:
                text = REPLACE_FIRST(text, match.full_text, result_part)
                remove_count += 1
            END IF
        END FOR
    END FOR
    
    RETURN text, remove_count
END FUNCTION

FUNCTION BREAK_SCENE_STRUCTURE(text):
    """打破场景完整结构（修复版：返回计数）"""
    scenes = DETECT_SCENES(text)
    break_count = 0
    
    FOR scene IN scenes:
        structure = ANALYZE_SCENE_STRUCTURE(scene)
        
        # 如果场景有完整的"起承转合"
        IF structure.has_all_four_parts AND RANDOM()< 0.3:
            # 随机删除"承"或"转"
            part_to_remove = RANDOM_CHOICE(["承", "转"])
            
            scene.content = REMOVE_STRUCTURE_PART(scene.content, part_to_remove)
            text = REPLACE_SCENE_IN_TEXT(text, scene)
            break_count += 1
        END IF
    END FOR
    
    RETURN text, break_count
END FUNCTION

FUNCTION ADD_CONVERSATIONAL_ROUGHNESS(text):
    """增加对话的"粗糙感"（修复版：返回计数）"""
    dialogues = EXTRACT_ALL_DIALOGUES(text)
    modify_count = 0
    
    FOR dialogue IN dialogues:
        # 30%概率做处理
        IF RANDOM() < 0.3:
            modification = RANDOM_CHOICE([
                "TRUNCATE",      # 说半句话
                "SINGLE_CHAR",   # 改成"嗯""啊""哦"
                "ADD_PAUSE"      # 添加"……"
            ])
            
            SWITCH modification:
                CASE "TRUNCATE":
                    # "你这是什么意思？" → "你这是什么——"
                    IF LENGTH(dialogue.content) > 10:
                        dialogue.content = TRUNCATE_AT_RANDOM(dialogue.content) + "——"
                        modify_count += 1
                    END IF
                
                CASE "SINGLE_CHAR":
                    # 完整回答 → "嗯"（仅对回应类对话）
                    IF LENGTH(dialogue.content) > 10 AND IS_RESPONSE_DIALOGUE(dialogue):
                        dialogue.content = RANDOM_CHOICE(["嗯", "啊", "哦", "呵"])
                        modify_count += 1
                    END IF
                
                CASE "ADD_PAUSE":
                    # 添加犹豫停顿
                    IF LENGTH(dialogue.content) > 15:
                        dialogue.content = INSERT_PAUSE_RANDOMLY(dialogue.content)
                        modify_count += 1
                    END IF
            END SWITCH
            
            text = REPLACE_DIALOGUE_IN_TEXT(text, dialogue)
        END IF
    END FOR
    
    RETURN text, modify_count
END FUNCTION

FUNCTION COMPRESS_LOW_DENSITY_PARAGRAPHS(text):
    """压缩低信息密度段落（修复版：返回计数）"""
    paragraphs = SPLIT_PARAGRAPHS(text)
    compress_count = 0
    
    FOR i, para IN ENUMERATE(paragraphs):
        IF LENGTH(para) < 100:
            CONTINUE
        END IF
        
        density = CALCULATE_INFO_DENSITY(para)
        
        # 信息密度<0.005（每200字不到1个信息点）
        IF density < 0.005:
            # 提取核心信息
            core = EXTRACT_CORE_INFO(para)
            
            IF LENGTH(core) < 20:
                # 核心信息太少，直接删除
                paragraphs[i] = ""
            ELSE:
                # 压缩到30-50字
                paragraphs[i] = REWRITE_BRIEFLY(core, 50)
            END IF
            
            compress_count += 1
        END IF
    END FOR
    
    RETURN JOIN(FILTER(paragraphs, lambda p: LENGTH(p) > 0), "\n\n"), compress_count
END FUNCTION

FUNCTION FIX_POV_SHIFTS(text, protagonist_name):
    """修复视角漂移（修复版：返回计数）"""
    thought_patterns = [
        r"(\\w{2,4})(想|觉得|认为|心想)",
        r"(\\w{2,4})的(内心|心里)"
    ]
    
    fix_count = 0
    
    FOR pattern IN thought_patterns:
        matches = FIND_ALL_WITH_CONTEXT(text, pattern)
        
        FOR match IN matches:
            character_name = EXTRACT_CHARACTER_NAME(match)
            
            # 如果不是主角的想法，删除
            IF character_name != protagonist_name AND character_name IS NOT NULL:
                text = REPLACE_FIRST(text, match.full_sentence, "")
                fix_count += 1
            END IF
        END FOR
    END FOR
    
    RETURN CLEAN_WHITESPACE(text), fix_count
END FUNCTION

FUNCTION COMPRESS_OVER_EXPANDED_INFO(text):
    """压缩过度展开的信息点（修复版：返回计数）"""
    info_points = DETECT_INFO_POINTS(text)
    compress_count = 0
    
    FOR point IN info_points:
        IF point.word_count > 150:
            # 提取核心
            core = EXTRACT_CORE_INFO(point.content)
            
            # 压缩到50-80字
            compressed = REWRITE_BRIEFLY(core, 80)
            
            text = REPLACE_FIRST(text, point.content, compressed)
            compress_count += 1
        END IF
    END FOR
    
    RETURN text, compress_count
END FUNCTION
```

---

## 【模块6】DIAGNOSE_CHAPTER_V3 - 全局诊断

```python
FUNCTION DIAGNOSE_CHAPTER_V3(chapter_content, parsed_data, monitors):
    """
    全局诊断（v3修复版）
    输出分层报告，快速定位问题
    """
    
    diagnosis = {
        "passed": TRUE,
        "has_critical_issues": FALSE,
        "summary": {},      # 快速摘要
        "critical": [],     # 必须修复
        "warnings": [],     # 建议优化
        "details": {},      # 详细数据
        "forced_delivery": FALSE
    }
    
    # ========== 计算统计数据 ==========
    stats = {
        "word_count": LENGTH(chapter_content),
        "dialogue_ratio": CALCULATE_DIALOGUE_RATIO(chapter_content),
        "inner_ratio": CALCULATE_INNER_MONOLOGUE_RATIO(chapter_content),
        "paragraph_count": COUNT_PARAGRAPHS(chapter_content),
        "avg_para_length": AVG_PARAGRAPH_LENGTH(chapter_content),
        "info_density": CALCULATE_INFO_DENSITY(chapter_content),
        "disease_score": CALCULATE_DISEASE_SCORE(chapter_content, parsed_data),
        "scene_checks": monitors.scene_checks
    }
    
    # ========== 快速摘要 ==========
    quality_score = CALCULATE_QUALITY_SCORE(stats)
    
    diagnosis.summary = {
        "status": "✅ 通过" IF diagnosis.passed ELSE "❌ 未通过",
        "word_count": f"{stats.word_count}字",
        "quality_score": f"{quality_score}/100"
    }
    
    # ========== 关键检查 ==========
    
    # 检查1：字数范围（硬约束）
    IF stats.word_count < WORD_COUNT_MIN:
        diagnosis.critical.APPEND({
            "type": "WORD_COUNT",
            "issue": f"字数不足（{stats.word_count}字，最低{WORD_COUNT_MIN}字）",
            "fix": "需要扩充关键情节或增加冲突",
            "severity": 10
        })
        diagnosis.passed = FALSE
        diagnosis.has_critical_issues = TRUE
    
    ELSE IF stats.word_count > WORD_COUNT_MAX:
        diagnosis.critical.APPEND({
            "type": "WORD_COUNT",
            "issue": f"字数超标（{stats.word_count}字，上限{WORD_COUNT_MAX}字）",
            "fix": "需要删减次要情节或压缩描写",
            "severity": 10
        })
        diagnosis.passed = FALSE
        diagnosis.has_critical_issues = TRUE
    END IF
    
    # 检查2：核心任务（必须完成）
    IF "core_mission" IN parsed_data.goals:
        core_mission = parsed_data.goals.core_mission
        
        IF NOT CHECK_MISSION_COMPLETED(chapter_content, core_mission, parsed_data):
            diagnosis.critical.APPEND({
                "type": "MISSION",
                "issue": f"核心任务未完成：{core_mission}",
                "fix": "必须重写或补充关键情节",
                "severity": 10
            })
            diagnosis.passed = FALSE
            diagnosis.has_critical_issues = TRUE
        END IF
    END IF
    
    # 检查3：红线违反（零容忍）
    IF "redlines" IN parsed_data.constraints:
        FOR redline IN parsed_data.constraints.redlines:
            IF CHECK_REDLINE_VIOLATION(chapter_content, redline):
                diagnosis.critical.APPEND({
                    "type": "REDLINE",
                    "issue": f"违反红线：{redline}",
                    "fix": "必须删除违规内容",
                    "severity": 15  # 最高严重度
                })
                diagnosis.passed = FALSE
                diagnosis.has_critical_issues = TRUE
            END IF
        END FOR
    END IF
    
    # 检查4：必须埋的钩子（重要但非关键）
    IF "hooks_to_plant" IN parsed_data.goals:
        FOR hook IN parsed_data.goals.hooks_to_plant:
            IF NOT CHECK_HOOK_PLANTED(chapter_content, hook):
                diagnosis.warnings.APPEND({
                    "type": "HOOK",
                    "issue": f"钩子未埋：{hook}",
                    "fix": "建议补充伏笔"
                })
            END IF
        END FOR
    END IF
    
    # ========== 警告项 ==========
    
    # 警告1：对话占比
    dialogue_ratio = stats.dialogue_ratio
    target_range = TOMATO_CORE_RULES.dialogue_ratio
    
    IF dialogue_ratio < target_range[0] * 0.8 OR dialogue_ratio > target_range[1] * 1.2:
        diagnosis.warnings.APPEND({
            "type": "DIALOGUE_RATIO",
            "issue": f"对话占比偏离目标（当前{dialogue_ratio*100:.1f}%，目标{target_range[0]*100}-{target_range[1]*100}%）",
            "fix": "调整对话/描写比例"
        })
    END IF
    
    # 警告2：Claude病症
    IF stats.disease_score > 30:  # 加权分数阈值
        diagnosis.warnings.APPEND({
            "type": "DISEASE",
            "issue": f"Claude病症加权分数过高（{stats.disease_score:.1f}分）",
            "fix": "已自动润色，但建议人工复查"
        })
    END IF
    
    # 警告3：信息密度
    IF stats.info_density < TOMATO_CORE_RULES.info_density_min:
        diagnosis.warnings.APPEND({
            "type": "INFO_DENSITY",
            "issue": f"信息密度过低（{stats.info_density:.3f}，最低{TOMATO_CORE_RULES.info_density_min}）",
            "fix": "压缩描写或增加信息点"
        })
    END IF
    
    # 警告4：内心戏过多
    IF stats.inner_ratio > TOMATO_CORE_RULES.inner_monologue_max * 1.2:
        diagnosis.warnings.APPEND({
            "type": "INNER_MONOLOGUE",
            "issue": f"内心戏占比过高（{stats.inner_ratio*100:.1f}%，上限{TOMATO_CORE_RULES.inner_monologue_max*100}%）",
            "fix": "已自动压缩，请复查"
        })
    END IF
    
    # 警告5：段落过长
    IF stats.avg_para_length > TOMATO_CORE_RULES.paragraph_max_length * 1.2:
        diagnosis.warnings.APPEND({
            "type": "PARAGRAPH_LENGTH",
            "issue": f"平均段落长度过长（{stats.avg_para_length:.0f}字，上限{TOMATO_CORE_RULES.paragraph_max_length}字）",
            "fix": "已自动压缩，请复查"
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
    
    # 字数：在目标范围内+10，严重偏离-20
    word_count = stats.word_count
    IF word_count < WORD_COUNT_MIN OR word_count > WORD_COUNT_MAX:
        score -= 20
    ELSE IF ABS(word_count - WORD_COUNT_TARGET) < 500:
        score += 10
    ELSE:
        score -= 5
    END IF
    
    # 对话占比：在目标范围内+15
    dialogue_ratio = stats.dialogue_ratio
    target_range = TOMATO_CORE_RULES.dialogue_ratio
    
    IF dialogue_ratio >= target_range[0] AND dialogue_ratio <= target_range[1]:
        score += 15
    ELSE IF dialogue_ratio >= target_range[0] * 0.8 AND dialogue_ratio <= target_range[1] * 1.2:
        score += 5
    ELSE:
        score -= 10
    END IF
    
    # 信息密度：达标+15
    IF stats.info_density >= TOMATO_CORE_RULES.info_density_min:
        score += 15
    ELSE:
        score -= 10
    END IF
    
    # Claude病症：每分-0.5分（加权后）
    score -= stats.disease_score * 0.5
    
    # 段落长度：平均<150字+10分
    IF stats.avg_para_length <= TOMATO_CORE_RULES.paragraph_max_length:
        score += 10
    ELSE:
        score -= 5
    END IF
    
    # 内心戏：不超标+10分
    IF stats.inner_ratio <= TOMATO_CORE_RULES.inner_monologue_max:
        score += 10
    ELSE:
        score -= 10
    END IF
    
    RETURN CLAMP(score, 0, 100)
END FUNCTION

FUNCTION CALCULATE_DISEASE_SCORE(text, parsed_data):
    """
    计算Claude病症加权分数
    """
    disease_score = 0
    
    # D1: 过度展开
    over_expanded = COUNT_OVER_EXPANDED_INFO(text, 150)
    disease_score += over_expanded * DISEASE_WEIGHTS["D1_过度展开"]
    
    # D2: 过度解释
    explain_patterns = ["这是因为", "之所以", "原因在于"]
    FOR pattern IN explain_patterns:
        disease_score += COUNT_OCCURRENCES(text, pattern) * DISEASE_WEIGHTS["D2_过度解释"]
    END FOR
    
    # D3: 过度内心戏
    inner_ratio = CALCULATE_INNER_MONOLOGUE_RATIO(text)
    IF inner_ratio > TOMATO_CORE_RULES.inner_monologue_max:
        excess = (inner_ratio - TOMATO_CORE_RULES.inner_monologue_max) * 100
        disease_score += excess * DISEASE_WEIGHTS["D3_过度内心戏"]
    END IF
    
    # D4: 过度安全
    hedging = ["可以说", "基本上", "在某种意义上"]
    FOR phrase IN hedging:
        disease_score += COUNT_OCCURRENCES(text, phrase) * DISEASE_WEIGHTS["D4_过度安全"]
    END FOR
    
    # D5: 过度冗余
    disease_score += DETECT_REPETITION_COUNT(text) * DISEASE_WEIGHTS["D5_过度冗余"]
    
    # D7: 过度逻辑
    causality = ["因为", "所以", "由于", "导致"]
    FOR word IN causality:
        disease_score += COUNT_OCCURRENCES(text, word) * DISEASE_WEIGHTS["D7_过度逻辑"]
    END FOR
    
    # D8: 过度完整
    disease_score += COUNT_COMPLETE_STRUCTURES(text) * DISEASE_WEIGHTS["D8_过度完整"]
    
    # D11: 信息密度不足
    low_density_paras = FIND_LOW_DENSITY_PARAGRAPHS(text, 0.005)
    disease_score += LENGTH(low_density_paras) * DISEASE_WEIGHTS["D11_信息密度不足"]
    
    # D12: 口水词
    filler_words = ["似乎", "仿佛", "或许", "可能", "大概", "好像"]
    FOR word IN filler_words:
        disease_score += COUNT_OCCURRENCES(text, word) * DISEASE_WEIGHTS["D12_口水词滥用"]
    END FOR
    
    # D13: 套路转折
    cliche_words = ["然而", "但是", "就在这时", "突然", "忽然"]
    FOR word IN cliche_words:
        disease_score += COUNT_OCCURRENCES(text, word) * DISEASE_WEIGHTS["D13_套路化转折"]
    END FOR
    
    # D14: 过度铺垫
    disease_score += DETECT_OVER_SETUP_COUNT(text) * DISEASE_WEIGHTS["D14_过度铺垫"]
    
    # D15: 结尾总结癖
    summary_patterns = ["他明白了", "他意识到", "他决定了"]
    FOR pattern IN summary_patterns:
        disease_score += COUNT_OCCURRENCES(text, pattern) * DISEASE_WEIGHTS["D15_结尾总结癖"]
    END FOR
    
    # D16: 不敢留白
    hint_patterns = ["似乎预示着", "可能会", "或许意味着"]
    FOR pattern IN hint_patterns:
        disease_score += COUNT_OCCURRENCES(text, pattern) * DISEASE_WEIGHTS["D16_不敢留白"]
    END FOR
    
    # D17: 不敢跳跃
    transition_phrases = ["过了", "来到", "走到", "片刻之后"]
    FOR phrase IN transition_phrases:
        disease_score += COUNT_OCCURRENCES(text, phrase) * DISEASE_WEIGHTS["D17_不敢跳跃"]
    END FOR
    
    # D18: 情绪词滥用
    emotion_words = ["震惊", "恐惧", "愤怒", "焦虑", "紧张"]
    FOR word IN emotion_words:
        disease_score += COUNT_OCCURRENCES(text, word) * DISEASE_WEIGHTS["D18_情绪词滥用"]
    END FOR
    
    # D19: 动作链
    action_chains = DETECT_ACTION_CHAINS(text)
    long_chains = FILTER(action_chains, lambda chain: LENGTH(chain) > 4)
    disease_score += LENGTH(long_chains) * DISEASE_WEIGHTS["D19_动作链完整癖"]
    
    # D20: 视角漂移
    protagonist_name = parsed_data.characters.protagonist.GET("name", "主角")
    disease_score += COUNT_POV_SHIFTS(text, protagonist_name) * DISEASE_WEIGHTS["D20_视角漂移"]
    
    RETURN disease_score
END FUNCTION
```

---

## 【模块7】PROBLEM_ANALYSIS - 问题分析

```python
FUNCTION ANALYZE_WHAT_WENT_WRONG(diagnosis, parsed_data, chapter_content):
    """
    分析诊断失败的根本原因（修复版：增加上下文）
    避免盲目重写
    """
    
    analysis = {
        "root_cause": [],
        "affected_scenes": [],
        "fix_strategy": []
    }
    
    # ========== 分析Critical问题 ==========
    FOR issue IN diagnosis.critical:
        SWITCH issue.type:
            CASE "WORD_COUNT":
                IF "不足" IN issue.issue:
                    analysis.root_cause.APPEND("内容量不足")
                    analysis.fix_strategy.APPEND("展开重要情节至200-300字/场景")
                    analysis.fix_strategy.APPEND("增加1-2个冲突场景")
                ELSE:
                    analysis.root_cause.APPEND("内容过度展开")
                    analysis.fix_strategy.APPEND("压缩次要场景至300-500字")
                    analysis.fix_strategy.APPEND("删除不必要的描写")
                END IF
            
            CASE "MISSION":
                analysis.root_cause.APPEND("遗漏关键情节")
                
                # 找到核心任务相关的关键词
                mission = parsed_data.goals.core_mission
                mission_keywords = EXTRACT_KEYWORDS(mission)
                
                # 检查哪些场景缺少这些关键词
                missing_scenes = FIND_SCENES_MISSING_KEYWORDS(chapter_content, mission_keywords)
                analysis.affected_scenes.EXTEND(missing_scenes)
                
                analysis.fix_strategy.APPEND(f"在场景{missing_scenes}补充核心任务相关情节")
            
            CASE "REDLINE":
                analysis.root_cause.APPEND("逻辑错误或理解偏差")
                
                # 从章节内容中找到违规位置
                redline_text = issue.issue.REPLACE("违反红线：", "")
                violation_location = FIND_VIOLATION_LOCATION(chapter_content, redline_text)
                
                IF violation_location:
                    analysis.affected_scenes.APPEND(violation_location.scene_idx)
                    analysis.fix_strategy.APPEND(f"删除场景{violation_location.scene_idx}第{violation_location.para_idx}段的违规内容")
                ELSE:
                    analysis.fix_strategy.APPEND("全文检查并删除违规内容")
                END IF
            
            CASE "HOOK":
                analysis.root_cause.APPEND("遗漏伏笔")
                analysis.fix_strategy.APPEND("在适当场景补充伏笔")
        END SWITCH
    END FOR
    
    # ========== 分析Warning（辅助判断）==========
    warning_types = MAP(diagnosis.warnings, lambda w: w.type)
    
    IF "DIALOGUE_RATIO" IN warning_types:
        analysis.root_cause.APPEND("对话占比失衡")
        analysis.fix_strategy.APPEND("增加/减少对话至35%-45%")
    END IF
    
    IF "DISEASE" IN warning_types:
        analysis.root_cause.APPEND("Claude病症严重")
        analysis.fix_strategy.APPEND("写作时更注重Show而非Tell")
    END IF
    
    IF "INFO_DENSITY" IN warning_types:
        analysis.root_cause.APPEND("信息密度不足")
        analysis.fix_strategy.APPEND("压缩描写或增加信息点")
    END IF
    
    # ========== 去重 ==========
    analysis.root_cause = UNIQUE(analysis.root_cause)
    analysis.affected_scenes = UNIQUE(analysis.affected_scenes)
    analysis.fix_strategy = UNIQUE(analysis.fix_strategy)
    
    PRINT "[ANALYSIS] 问题根源: {JOIN(analysis.root_cause, ', ')}"
    PRINT "[ANALYSIS] 受影响场景: {analysis.affected_scenes}"
    PRINT "[ANALYSIS] 修复策略: {LENGTH(analysis.fix_strategy)}项"
    
    RETURN analysis
END FUNCTION

FUNCTION GENERATE_FIX_INSTRUCTION(problem_analysis):
    """
    根据问题分析生成修正指令（修复版：结构化）
    """
    
    fix_instruction = {
        "focus_scenes": problem_analysis.affected_scenes,
        "must_do": [],
        "must_avoid": [],
        "target_adjustments": {}
    }
    
    # ========== 处理根本原因 ==========
    FOR cause IN problem_analysis.root_cause:
        SWITCH cause:
            CASE "内容量不足":
                fix_instruction.must_do.APPEND("展开重要情节至200-300字")
                fix_instruction.must_do.APPEND("增加1-2个冲突场景")
                fix_instruction.target_adjustments["min_scene_words"] = 400
            
            CASE "内容过度展开":
                fix_instruction.must_do.APPEND("压缩次要场景至300-500字")
                fix_instruction.must_avoid.APPEND("过度描写环境和细节")
                fix_instruction.target_adjustments["max_scene_words"] = 800
            
            CASE "遗漏关键情节":
                fix_instruction.must_do.APPEND("补充核心任务相关场景")
                fix_instruction.must_do.APPEND("确保核心任务在文中明确体现")
            
            CASE "逻辑错误":
                fix_instruction.must_avoid.APPEND("违反红线的内容")
                fix_instruction.must_do.APPEND("重新检查逻辑合理性")
            
            CASE "对话占比失衡":
                fix_instruction.target_adjustments["dialogue_ratio"] = [0.35, 0.45]
                fix_instruction.must_do.APPEND("调整对话/描写比例")
            
            CASE "Claude病症严重":
                fix_instruction.must_avoid.APPEND("口水词、套路转折、情绪词直写")
                fix_instruction.must_do.APPEND("用Show代替Tell")
            
            CASE "信息密度不足":
                fix_instruction.must_do.APPEND("增加信息点密度至0.01/100字")
                fix_instruction.must_avoid.APPEND("空洞的描写和铺垫")
        END SWITCH
    END FOR
    
    # ========== 应用修复策略 ==========
    FOR strategy IN problem_analysis.fix_strategy:
        IF strategy NOT IN fix_instruction.must_do:
            fix_instruction.must_do.APPEND(strategy)
        END IF
    END FOR
    
    PRINT "[FIX] 生成修正指令: {LENGTH(fix_instruction.must_do)}项必做，{LENGTH(fix_instruction.must_avoid)}项禁止"
    
    RETURN fix_instruction
END FUNCTION
```

---

## 【模块8】EMOTION_WRITER_V3 - 情绪写作器

```python
FUNCTION WRITE_EMOTION(unit, expansion_level, parsed_data):
    """
    写情绪单元（v3修复版）
    根据expansion_level动态Show/Tell
    """
    
    emotion_text = ""
    emotion_type = unit.GET("emotion_type", "焦虑")
    emotion_intensity = unit.GET("intensity", 60)
    
    # ========== 根据expansion_level决定Show/Tell ==========
    SWITCH expansion_level:
        CASE "EXPAND":
            # 详写：生理反应 + 简短内心戏
            micro_expressions = GENERATE_MICRO_EXPRESSIONS(emotion_type, emotion_intensity)
            emotion_text = WRITE_MICRO_EXPRESSIONS(micro_expressions)
            
            # 加一句内心戏（<30字）
            IF "active_thought" IN parsed_data.emotions.GET("protagonist", {}):
                inner = parsed_data.emotions.protagonist.active_thought
                IF LENGTH(inner) > 0:
                    emotion_text += f""{inner[:30]}""
                END IF
            END IF
        
        CASE "BRIEF":
            # 简写：只写生理反应（取2个）
            micro_expressions = GENERATE_MICRO_EXPRESSIONS(emotion_type, emotion_intensity)
            emotion_text = WRITE_MICRO_EXPRESSIONS(micro_expressions[:2])
        
        CASE "SKIP":
            # 跳过：不写情绪
            emotion_text = ""
    END SWITCH
    
    RETURN emotion_text
END FUNCTION

FUNCTION GENERATE_MICRO_EXPRESSIONS(emotion_type, intensity):
    """生成生理反应（修复版：返回列表）"""
    emotion_map = {
        "焦虑": ["胃部收紧", "呼吸变浅", "手心冒汗", "肩膀紧绷"],
        "恐惧": ["后背发凉", "腿发软", "瞳孔收缩", "心跳骤停"],
        "愤怒": ["太阳穴跳动", "拳头攥紧", "脸颊发烫", "牙关紧咬"],
        "兴奋": ["心跳加快", "呼吸急促", "眼睛发亮", "血液上涌"],
        "悲伤": ["鼻腔发酸", "眼眶发热", "喉头发紧", "胸口发闷"],
        "紧张": ["喉咙发干", "心跳加速", "肌肉紧绷", "手指颤抖"],
        "困惑": ["眉头紧锁", "目光游移", "嘴唇微张", "身体僵住"],
        "惊讶": ["瞳孔放大", "呼吸一窒", "身体后仰", "眼睛瞪圆"]
    }
    
    reactions = emotion_map.GET(emotion_type, ["身体紧绷", "呼吸不稳"])
    
    # 根据强度选择数量
    IF intensity > 70:
        RETURN reactions  # 全部
    ELSE IF intensity > 40:
        RETURN reactions[:3]  # 前3个
    ELSE:
        RETURN reactions[:1]  # 前1个
    END IF
END FUNCTION

FUNCTION WRITE_MICRO_EXPRESSIONS(reactions):
    """将生理反应写成文本"""
    IF LENGTH(reactions) == 0:
        RETURN ""
    END IF
    
    IF LENGTH(reactions) == 1:
        RETURN f"他的{reactions[0]}。"
    ELSE IF LENGTH(reactions) == 2:
        RETURN f"他的{reactions[0]}，{reactions[1]}。"
    ELSE:
        # 3个以上，用顿号连接
        all_but_last = JOIN(reactions[:-1], "、")
        RETURN f"他的{all_but_last}，{reactions[-1]}。"
    END IF
END FUNCTION
```

---

## 【模块9】DELIVERY_V3 - 交付协议

```python
FUNCTION DELIVER_OUTPUT_V3(chapter_content, new_facts, diagnosis, monitors):
    """
    交付输出（v3修复版：结构化输出）
    """
    
    output = {}
    
    # ========== 输出1：章节正文 ==========
    output["chapter_content"] = chapter_content
    
    # ========== 输出2：快速摘要 ==========
    status_emoji = "✅" IF diagnosis.passed ELSE "❌"
    forced_note = " [强制交付]" IF diagnosis.forced_delivery ELSE ""
    
    output["summary"] = f"""
## 快速摘要
{status_emoji} {diagnosis.summary.status}{forced_note}
- 字数: {diagnosis.summary.word_count}
- 质量分: {diagnosis.summary.quality_score}
"""
    
    # ========== 输出3：问题清单（如果有）==========
    IF LENGTH(diagnosis.critical) > 0:
        output["summary"] += "\n### ⚠️ 必须修复\n"
        FOR issue IN diagnosis.critical:
            output["summary"] += f"- **[{issue.type}]** {issue.issue}\n"
            output["summary"] += f"  修复: {issue.fix}\n"
        END FOR
    END IF
    
    IF LENGTH(diagnosis.warnings) > 0:
        output["summary"] += "\n### 💡 建议优化\n"
        FOR warning IN diagnosis.warnings:
            output["summary"] += f"- **[{warning.type}]** {warning.issue}\n"
        END FOR
    END IF
    
    # ========== 输出4：新增Fact清单 ==========
    output["new_facts"] = FORMAT_FACTS_LIST(new_facts)
    
    # ========== 输出5：详细诊断（可折叠）==========
    details_content = f"""
- 总字数: {diagnosis.details.word_count}
- 对话占比: {diagnosis.details.dialogue_ratio * 100:.1f}% (目标{TOMATO_CORE_RULES.dialogue_ratio[0]*100}-{TOMATO_CORE_RULES.dialogue_ratio[1]*100}%)
- 内心戏占比: {diagnosis.details.inner_ratio * 100:.1f}% (上限{TOMATO_CORE_RULES.inner_monologue_max*100}%)
- 段落数: {diagnosis.details.paragraph_count}
- 平均段落长度: {diagnosis.details.avg_para_length:.0f}字 (上限{TOMATO_CORE_RULES.paragraph_max_length}字)
- 信息密度: {diagnosis.details.info_density:.3f} (最低{TOMATO_CORE_RULES.info_density_min})
- Claude病症加权分: {diagnosis.details.disease_score:.1f}

### 场景检查记录
"""
    
    FOR check IN monitors.scene_checks:
        status_icon = "✅" IF check.severity == "OK" ELSE ("⚠️" IF check.severity == "WARNING" ELSE "❌")
        details_content += f"- 场景{check.scene_idx}: {status_icon} {check.word_count}字"
        
        IF check.issue:
            details_content += f" ({check.issue})"
        END IF
        
        details_content += "\n"
    END FOR
    
    output["details"] = f"""
<details>
<summary>📊 详细统计（点击展开）</summary>

{details_content}
</details>
"""
    
    RETURN output
END FUNCTION
```

---

## 【模块10】UTILITY_FUNCTIONS - 工具函数库（新增完整实现）

```python
# ==================== 胶囊解析 ====================

FUNCTION PARSE_CAPSULE(CAPSULE):
    """
    解析信息胶囊，提取所有模块的数据
    返回结构化的数据对象
    """
    
    parsed = {
        "meta": {},           # 元信息（章节号、标题等）
        "context": {},        # 上下文（时空、主角快照等）
        "goals": {},          # 目标（核心任务、钩子等）
        "characters": {},     # 角色档案
        "relationships": {},  # 关系温度
        "emotions": {},       # 情绪数据
        "constraints": {},    # 约束（红线、禁止清单等）
        "world": {},          # 世界观
        "resources": {},      # 资源清单
        "style": {},          # 语言风格
        "sensors": {},        # 感官素材
        "risks": {},          # 高危风险
        "prev_chapter": "",   # 上章正文
        "info_points": [],    # 必须埋的信息点
        "hooks": []           # 未完成的钩子
    }
    
    # 解析§1 此时此刻
    parsed.context.time = EXTRACT(CAPSULE, "§1", "时间")
    parsed.context.location = EXTRACT(CAPSULE, "§1", "地点")
    parsed.context.atmosphere = EXTRACT(CAPSULE, "§1", "天气/光线/声音")
    parsed.context.protagonist_posture = EXTRACT(CAPSULE, "§1", "姿态")
    parsed.context.protagonist_body = EXTRACT(CAPSULE, "§1", "身体")
    parsed.context.protagonist_focus = EXTRACT(CAPSULE, "§1", "注意力焦点")
    parsed.context.protagonist_emotion = EXTRACT(CAPSULE, "§1", "主导情绪")
    
    # 解析§2 上章接口
    parsed.prev_chapter = EXTRACT(CAPSULE, "§19", "上章完整正文")
    parsed.context.connection_instruction = EXTRACT(CAPSULE, "§2.2", "本章衔接说明")
    parsed.context.body_memory = EXTRACT(CAPSULE, "§2.3", "身体记忆")
    parsed.context.emotion_residue = EXTRACT(CAPSULE, "§2.4", "情感余温")
    parsed.context.unfinished_thoughts = EXTRACT(CAPSULE, "§2.5", "未完成的念头")
    parsed.context.avoid_breaks = EXTRACT(CAPSULE, "§2.6", "必须避免的断裂")
    
    # 解析§3 本章要完成什么
    parsed.goals.core_mission = EXTRACT(CAPSULE, "§3", "核心任务")
    parsed.goals.wildcard = EXTRACT(CAPSULE, "§3", "疯狂分支")
    parsed.goals.hooks_to_plant = EXTRACT(CAPSULE, "§3", "本章必须埋下的钩子")
    parsed.goals.cost_risk = EXTRACT(CAPSULE, "§3", "本章代价/风险")
    parsed.goals.failure_consequence = EXTRACT(CAPSULE, "§3", "本章失败后果")
    parsed.goals.emotional_goal = EXTRACT(CAPSULE, "§3", "情感目标")
    parsed.goals.conflict_suggestions = EXTRACT(CAPSULE, "§3", "冲突建议")
    parsed.goals.absolute_no = EXTRACT(CAPSULE, "§3", "绝对禁止")
    
    # 解析§4 关系温度
    parsed.relationships = PARSE_RELATIONSHIP_TABLE(CAPSULE, "§4")
    
    # 解析§5 角色内心世界
    parsed.emotions.protagonist = {
        "micro_expressions": EXTRACT(CAPSULE, "§5", "此刻的显微镜特写"),
        "dominant_emotion": EXTRACT(CAPSULE, "§5", "此刻的主导情绪"),
        "active_thought": EXTRACT(CAPSULE, "§5", "此刻脑子里最活跃的念头"),
        "core_conflict": EXTRACT(CAPSULE, "§5", "此刻的核心冲突"),
        "behavior_motivation": EXTRACT(CAPSULE, "§5", "行为的情感动机")
    }
    
    # 解析§6 情绪的可能性
    parsed.emotions.start_emotion = EXTRACT(CAPSULE, "§6", "开篇情绪")
    parsed.emotions.lower_bound = EXTRACT(CAPSULE, "§6", "最低不跌破")
    parsed.emotions.upper_bound = EXTRACT(CAPSULE, "§6", "最高可以到")
    parsed.emotions.fuel_elements = EXTRACT(CAPSULE, "§6", "情绪燃料")
    
    # 解析§7 主角档案
    parsed.characters.protagonist = {
        "name": EXTRACT(CAPSULE, "§7", "姓名"),
        "current_situation": EXTRACT(CAPSULE, "§7", "本章处境"),
        "mbti": EXTRACT(CAPSULE, "§7", "MBTI"),
        "core_drive": EXTRACT(CAPSULE, "§7", "核心驱动力"),
        "decision_instinct": EXTRACT(CAPSULE, "§7", "决策本能"),
        "ooc_redlines": EXTRACT(CAPSULE, "§7", "绝对不会做的事"),
        "speech_style": EXTRACT(CAPSULE, "§7", "言行标签"),
        "ability_boundary": EXTRACT(CAPSULE, "§7", "能力边界")
    }
    
    # 解析§8 配角档案
    parsed.characters.supporting = PARSE_SUPPORTING_CHARACTERS(CAPSULE, "§8")
    
    # 解析§9 记忆库
    parsed.info_points = EXTRACT(CAPSULE, "§9", "本章必须埋下的信息点")
    parsed.context.relevant_facts = EXTRACT(CAPSULE, "§9", "本章相关的Fact")
    parsed.hooks = EXTRACT(CAPSULE, "§9", "未完成的钩子")
    parsed.context.urgent_problems = EXTRACT(CAPSULE, "§9", "当前最紧迫的问题")
    parsed.context.info_asymmetry = EXTRACT(CAPSULE, "§9", "信息不对称")
    
    # 解析§10 世界观
    parsed.world = PARSE_WORLDVIEW_SETTINGS(CAPSULE, "§10")
    
    # 解析§11 资源清单
    parsed.resources = PARSE_RESOURCES(CAPSULE, "§11")
    
    # 解析§12 语言风格锚点
    parsed.style = PARSE_STYLE_TABLE(CAPSULE, "§12")
    
    # 解析§13 感官素材库
    parsed.sensors = PARSE_SENSOR_MATERIALS(CAPSULE, "§13")
    
    # 解析§14-15 红线和禁止清单
    parsed.constraints.redlines = EXTRACT(CAPSULE, "§14", "绝对红线")
    parsed.constraints.prohibitions = EXTRACT(CAPSULE, "§15", "禁止清单汇总")
    
    # 解析§16 铺垫任务
    parsed.goals.foreshadowing = EXTRACT(CAPSULE, "§16", "为下一章埋钉子")
    
    # 解析§17 补充信息
    parsed.meta.additional_info = EXTRACT(CAPSULE, "§17", "补充信息")
    
    # 解析§18 高危风险预警
    parsed.risks = PARSE_HIGH_RISKS(CAPSULE, "§18")
    
    RETURN parsed
END FUNCTION

FUNCTION VALIDATE_CAPSULE_INTEGRITY(parsed_data):
    """
    校验胶囊数据完整性
    """
    required_fields = [
        "context.time",
        "context.location",
        "goals.core_mission",
        "characters.protagonist",
        "constraints.redlines"
    ]
    
    FOR field IN required_fields:
        IF NOT EXISTS(parsed_data, field):
            THROW ERROR "胶囊数据缺失关键字段: {field}"
        END IF
    END FOR
    
    PRINT "[VALIDATION] 胶囊数据完整性校验通过"
END FUNCTION



# ==================== 场景估算 ====================

FUNCTION ESTIMATE_SCENE_COUNT(parsed_data):
    """
    从Capsule推断场景数量
    规则：3000-4000字 → 4-5个场景
    """
    word_target = parsed_data.meta.GET("word_count_target", WORD_COUNT_TARGET)
    scene_count = ROUND(word_target / SCENE_WORD_AVG)
    
    # 约束在合理范围（最少3场景，最多8场景）
    scene_count = CLAMP(scene_count, 3, 8)
    
    RETURN scene_count
END FUNCTION

FUNCTION GET_SCENE_IMPORTANCES(parsed_data, scene_count):
    """
    获取每个场景的重要性（1-10分）
    """
    importances = {}
    
    # 如果Capsule中明确指定了场景重要性，使用指定值
    IF "scene_importances" IN parsed_data.meta:
        RETURN parsed_data.meta.scene_importances
    END IF
    
    # 否则使用默认规则：
    # 开篇和结尾最重要(8分)，中间递增(5→7分)
    FOR i = 1 TO scene_count:
        IF i == 1 OR i == scene_count:
            importances[i] = 8  # 开篇和结尾
        ELSE IF i <= scene_count / 2:
            importances[i] = 5  # 前半部分
        ELSE:
            importances[i] = 7  # 后半部分（递增）
        END IF
    END FOR
    
    RETURN importances
END FUNCTION

# ==================== 爽点相关 ====================

FUNCTION SELECT_COOL_POINT_TYPE_FOR_SCENE(scene_idx, parsed_data, scene_count):
    """为场景选择合适的爽点类型"""
    
    # 根据场景位置推断合适的爽点
    position_ratio = scene_idx / scene_count
    
    IF position_ratio < 0.3:
        # 前30%：装逼爽或打脸爽
        RETURN RANDOM_CHOICE(["装逼爽", "打脸爽"])
    
    ELSE IF position_ratio < 0.7:
        # 中段：复仇爽或升级爽
        RETURN RANDOM_CHOICE(["复仇爽", "升级爽"])
    
    ELSE:
        # 后30%：反杀爽
        RETURN "反杀爽"
    END IF
END FUNCTION

FUNCTION GENERATE_COOL_POINT(cool_type, parsed_data, scene_idx):
    """生成爽点文本"""
    cool_config = COOL_POINT_TYPES[cool_type]
    structure_parts = SPLIT(cool_config.结构, " → ")
    
    cool_text = ""
    
    FOR part IN structure_parts:
        phase_text = WRITE_COOL_PHASE(part, parsed_data, cool_type)
        cool_text += phase_text + "\n\n"
    END FOR
    
    RETURN cool_text
END FUNCTION

FUNCTION INSERT_COOL_POINT(scene_text, cool_text, cool_type):
    """
    插入爽点到场景中（修复版：明确位置）
    """
    insert_position = COOL_POINT_TYPES[cool_type].插入位置
    
    SWITCH insert_position:
        CASE "冲突后":
            # 找到冲突结束的位置
            conflict_end = FIND_CONFLICT_END(scene_text)
            IF conflict_end > 0:
                RETURN scene_text[:conflict_end] + "\n\n" + cool_text + scene_text[conflict_end:]
            END IF
        
        CASE "场景高潮":
            # 找到场景的高潮位置（70%处）
            insert_pos = ROUND(LENGTH(scene_text) * 0.7)
            # 找到最近的段落结束
            para_end = FIND_NEXT(scene_text, "\n\n", insert_pos)
            IF para_end > 0:
                RETURN scene_text[:para_end] + "\n\n" + cool_text + scene_text[para_end:]
            END IF
        
        CASE "场景中段":
            # 50%位置插入
            insert_pos = ROUND(LENGTH(scene_text) * 0.5)
            para_end = FIND_NEXT(scene_text, "\n\n", insert_pos)
            IF para_end > 0:
                RETURN scene_text[:para_end] + "\n\n" + cool_text + scene_text[para_end:]
            END IF
        
        CASE "场景末尾":
            # 直接追加到场景末尾
            RETURN scene_text + "\n\n" + cool_text
    END SWITCH
    
    # 默认：插入到场景中段
    mid_pos = ROUND(LENGTH(scene_text) * 0.5)
    RETURN scene_text[:mid_pos] + "\n\n" + cool_text + scene_text[mid_pos:]
END FUNCTION

# ==================== 伏笔相关 ====================

FUNCTION CHOOSE_SCENE_FOR_FORESHADOW(foreshadow, scene_count, parsed_data):
    """选择埋伏笔的场景"""
    # 如果Capsule中指定了场景，使用指定值
    IF "target_scene" IN foreshadow:
        RETURN foreshadow.target_scene
    END IF
    
    # 否则根据伏笔类型选择：
    # 显眼度高的放在后半部分，隐蔽的放在前半部分
    visibility = foreshadow.GET("visibility", "隐蔽")
    
    IF visibility IN ["极度显眼", "显眼"]:
        # 后半部分（60%-80%）
        RETURN RANDOM_INT(CEIL(scene_count * 0.6), CEIL(scene_count * 0.8))
    ELSE:
        # 前半部分（20%-50%）
        RETURN RANDOM_INT(CEIL(scene_count * 0.2), CEIL(scene_count * 0.5))
    END IF
END FUNCTION

FUNCTION WRITE_FORESHADOW(foreshadow, parsed_data):
    """
    写伏笔/钉子/铺垫（根据显眼度）
    """
    visibility = foreshadow.GET("visibility", "隐蔽")
    content = foreshadow.GET("content", "")
    
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
            detail = ADD_MINOR_DETAIL(content, parsed_data)
            RETURN f"{content}，{detail}。"
        
        CASE "显眼":
            # 单独成段
            RETURN f"\n\n{content}。\n\n"
        
        CASE "极度显眼":
            # 强调 + 主角注意到
            reaction = GENERATE_PROTAGONIST_REACTION(content, parsed_data)
            RETURN f"\n\n{content}。{reaction}\n\n"
    END SWITCH
END FUNCTION

FUNCTION INSERT_FORESHADOW(scene_text, foreshadow_text, visibility):
    """
    插入伏笔到场景中
    """
    IF visibility IN ["极度隐蔽", "隐蔽"]:
        # 插入到场景前30%（不显眼位置）
        insert_pos = ROUND(LENGTH(scene_text) * 0.3)
    ELSE:
        # 插入到场景中段或后段（显眼位置）
        insert_pos = ROUND(LENGTH(scene_text) * 0.6)
    END IF
    
    # 找到最近的段落结束
    para_end = FIND_NEXT(scene_text, "\n\n", insert_pos)
    
    IF para_end > 0:
        RETURN scene_text[:para_end] + foreshadow_text + scene_text[para_end:]
    ELSE:
        # 找不到段落结束，插入到中间位置
        RETURN scene_text[:insert_pos] + foreshadow_text + scene_text[insert_pos:]
    END IF
END FUNCTION

# ==================== 场景分解 ====================

FUNCTION DECOMPOSE_SCENE_TO_UNITS(scene_idx, parsed_data, humanizer):
    """
    将场景拆解为写作单元
    """
    units = []
    
    # 单元1：场景开场（环境或动作）
    IF scene_idx == 1:
        # 开篇场景：根据开篇任务决定
        opening_task = FIND_TASK_BY_TYPE(humanizer.task_checklist.GET(scene_idx, []), "开篇")
        IF opening_task:
            # 开篇任务会处理，这里不加环境单元
            pass
        ELSE:
            units.APPEND({
                "type": "描写",
                "importance": 5,
                "content": "环境"
            })
        END IF
    ELSE:
        # 非开篇：简短环境描写（低重要性）
        units.APPEND({
            "type": "描写",
            "importance": 3,
            "content": "环境"
        })
    END IF
    
    # 单元2：主要动作/对话（从核心任务推断）
    IF "core_mission" IN parsed_data.goals:
        mission = parsed_data.goals.core_mission
        mission_units = INFER_UNITS_FROM_MISSION(mission, scene_idx, parsed_data)
        units.EXTEND(mission_units)
    END IF
    
    # 单元3：情绪反应（如果需要）
    IF scene_idx IN [1, ROUND(GET_SCENE_COUNT_FROM_CAPSULE(parsed_data) / 2)]:
        units.APPEND({
            "type": "情绪",
            "importance": 7,
            "emotion_type": parsed_data.emotions.GET("start_emotion", "焦虑"),
            "intensity": 60
        })
    END IF
    
    RETURN units
END FUNCTION

FUNCTION INFER_UNITS_FROM_MISSION(mission, scene_idx, parsed_data):
    """
    从核心任务推断场景需要的单元
    """
    units = []
    
    # 提取任务中的动作关键词
    action_keywords = EXTRACT_ACTION_KEYWORDS(mission)
    
    FOR keyword IN action_keywords:
        units.APPEND({
            "type": "动作",
            "importance": 8,
            "content": keyword
        })
    END FOR
    
    # 如果任务中包含对话关键词，加入对话单元
    IF CONTAINS_DIALOGUE_KEYWORDS(mission):
        units.APPEND({
            "type": "对话",
            "importance": 7,
            "content": "与关键角色对话"
        })
    END IF
    
    RETURN units
END FUNCTION

# ==================== 场景检测 ====================

FUNCTION DETECT_SCENES(text):
    """
    检测文本中的场景分割
    """
    # 按空行分割段落
    paragraphs = SPLIT_PARAGRAPHS(text)
    
    scenes = []
    current_scene = {
        "start_idx": 0,
        "content": "",
        "paragraphs": []
    }
    
    FOR i, para IN ENUMERATE(paragraphs):
        current_scene.paragraphs.APPEND(para)
        current_scene.content += para + "\n\n"
        
        # 检测场景分割信号：
        # 1. 时间跳跃词
        # 2. 空间转换词
        # 3. 明显的结束句
        IF HAS_SCENE_BREAK_SIGNAL(para) AND i < LENGTH(paragraphs) - 1:
            scenes.APPEND(current_scene)
            current_scene = {
                "start_idx": i + 1,
                "content": "",
                "paragraphs": []
            }
        END IF
    END FOR
    
    # 添加最后一个场景
    IF LENGTH(current_scene.paragraphs) > 0:
        scenes.APPEND(current_scene)
    END IF
    
    RETURN scenes
END FUNCTION

FUNCTION ANALYZE_SCENE_STRUCTURE(scene):
    """
    分析场景结构（起承转合）
    """
    paragraphs = scene.paragraphs
    para_count = LENGTH(paragraphs)
    
    structure = {
        "has_start": FALSE,      # 起
        "has_development": FALSE,  # 承
        "has_turn": FALSE,        # 转
        "has_conclusion": FALSE,  # 合
        "has_all_four_parts": FALSE
    }
    
    IF para_count < 4:
        RETURN structure
    END IF
    
    # 检测"起"：第一段是否有环境/动作开场
    IF HAS_OPENING_PATTERN(paragraphs[0]):
        structure.has_start = TRUE
    END IF
    
    # 检测"承"：中间段落是否有发展
    middle_paras = paragraphs[1:para_count-2]
    IF HAS_DEVELOPMENT_PATTERN(middle_paras):
        structure.has_development = TRUE
    END IF
    
    # 检测"转"：是否有转折词或冲突
    FOR para IN paragraphs:
        IF HAS_TURN_PATTERN(para):
            structure.has_turn = TRUE
            BREAK
        END IF
    END FOR
    
    # 检测"合"：最后一段是否有总结
    IF HAS_CONCLUSION_PATTERN(paragraphs[-1]):
        structure.has_conclusion = TRUE
    END IF
    
    # 判断是否完整
    structure.has_all_four_parts = (
        structure.has_start AND
        structure.has_development AND
        structure.has_turn AND
        structure.has_conclusion
    )
    
    RETURN structure
END FUNCTION

# ==================== 红线检查 ====================

FUNCTION CHECK_REDLINE_VIOLATION(text, redline):
    """检查是否违反红线（语义匹配）"""
    # 提取红线中的关键词
    keywords = EXTRACT_KEYWORDS(redline)
    
    # 快速筛选：所有关键词都存在才进一步检查
    FOR keyword IN keywords:
        IF keyword NOT IN text:
            RETURN FALSE
        END IF
    END FOR
    
    # 语义检查：找到包含关键词的句子，判断是否违规
    sentences_with_keywords = FIND_SENTENCES_CONTAINING_ALL(text, keywords)
    
    FOR sentence IN sentences_with_keywords:
        IF SEMANTIC_MATCH(sentence, redline):
            RETURN TRUE
        END IF
    END FOR
    
    RETURN FALSE
END FUNCTION

FUNCTION CONTAINS_REDLINE_KEYWORDS(text, parsed_data):
    """检查文本是否包含红线关键词"""
    IF "redlines" NOT IN parsed_data.constraints:
        RETURN FALSE
    END IF
    
    FOR redline IN parsed_data.constraints.redlines:
        keywords = EXTRACT_KEYWORDS(redline)
        IF ALL(keyword IN text FOR keyword IN keywords):
            RETURN TRUE
        END IF
    END FOR
    
    RETURN FALSE
END FUNCTION

# ==================== 任务检查 ====================

FUNCTION CHECK_MISSION_COMPLETED(text, mission, parsed_data):
    """检查核心任务是否完成"""
    # 提取任务关键词
    mission_keywords = EXTRACT_KEYWORDS(mission)
    
    # 所有关键词都必须出现
    FOR keyword IN mission_keywords:
        IF keyword NOT IN text:
            RETURN FALSE
        END IF
    END FOR
    
    # 语义验证：不仅关键词存在，还要确保完成了任务
    RETURN SEMANTIC_VERIFY_MISSION(text, mission, mission_keywords)
END FUNCTION

FUNCTION CONTAINS_CORE_MISSION_KEYWORDS(text, parsed_data):
    """检查文本是否包含核心任务关键词"""
    IF "core_mission" NOT IN parsed_data.goals:
        RETURN FALSE
    END IF
    
    mission = parsed_data.goals.core_mission
    keywords = EXTRACT_KEYWORDS(mission)
    
    RETURN ALL(keyword IN text FOR keyword IN keywords)
END FUNCTION

FUNCTION CHECK_HOOK_PLANTED(text, hook):
    """检查钩子是否埋下"""
    hook_keywords = EXTRACT_KEYWORDS(hook)
    RETURN ALL(keyword IN text FOR keyword IN hook_keywords)
END FUNCTION

# ==================== 信息密度计算 ====================

FUNCTION COUNT_NEW_INFO(text):
    """统计新信息点数量"""
    info_count = 0
    
    # 信息点1：专有名词（人名、地名、物品名）
    info_count += COUNT_PROPER_NOUNS(text)
    
    # 信息点2：剧情动词（发现、得到、失去、打败等）
    plot_verbs = ["发现", "得到", "失去", "打败", "逃离", "进入", "离开", "遇到"]
    FOR verb IN plot_verbs:
        info_count += COUNT_OCCURRENCES(text, verb)
    END FOR
    
    # 信息点3：对话回合（每2句对话算1个信息点）
    dialogue_exchanges = COUNT_DIALOGUE_EXCHANGES(text)
    info_count += ROUND(dialogue_exchanges / 2)
    
    # 信息点4：状态变化（情绪、位置、关系）
    info_count += COUNT_STATE_CHANGES(text)
    
    RETURN info_count
END FUNCTION

FUNCTION CALCULATE_INFO_DENSITY(text):
    """计算信息密度"""
    char_count = LENGTH(text)
    IF char_count == 0:
        RETURN 0
    END IF
    
    info_count = COUNT_NEW_INFO(text)
    RETURN info_count / char_count
END FUNCTION

# ==================== 对话相关 ====================

FUNCTION CALCULATE_DIALOGUE_RATIO(text):
    """计算对话占比"""
    dialogues = EXTRACT_ALL_DIALOGUES(text)
    
    dialogue_chars = 0
    FOR dialogue IN dialogues:
        dialogue_chars += LENGTH(dialogue.content)
    END FOR
    
    total_chars = LENGTH(text)
    
    IF total_chars == 0:
        RETURN 0
    END IF
    
    RETURN dialogue_chars / total_chars
END FUNCTION

FUNCTION EXTRACT_ALL_DIALOGUES(text):
    """提取所有对话"""
    # 匹配 "..." 或 "..." 格式
    pattern = r"[""]([^""]+)[""]"
    matches = FIND_ALL_WITH_CONTEXT(text, pattern)
    
    dialogues = []
    FOR match IN matches:
        dialogues.APPEND({
            "content": match.group(1),
            "full_text": match.group(0),
            "start_pos": match.start(),
            "end_pos": match.end()
        })
    END FOR
    
    RETURN dialogues
END FUNCTION

FUNCTION COUNT_DIALOGUE_EXCHANGES(text):
    """统计对话回合数"""
    dialogues = EXTRACT_ALL_DIALOGUES(text)
    RETURN LENGTH(dialogues)
END FUNCTION

# ==================== 内心戏相关 ====================

FUNCTION CALCULATE_INNER_MONOLOGUE_RATIO(text):
    """计算内心戏占比"""
    inner_segments = EXTRACT_INNER_MONOLOGUE(text)
    
    inner_chars = 0
    FOR segment IN inner_segments:
        inner_chars += LENGTH(segment.content)
    END FOR
    
    total_chars = LENGTH(text)
    
    IF total_chars == 0:
        RETURN 0
    END IF
    
    RETURN inner_chars / total_chars
END FUNCTION

FUNCTION EXTRACT_INNER_MONOLOGUE(text):
    """提取所有内心戏片段"""
    # 内心戏特征：
    # 1. "他想"、"他心想"、"他觉得"等
    # 2. "他暗自"、"他心里"等
    # 3. 问自己的问题
    
    inner_patterns = [
        r"(他|她)(想|心想|觉得|认为|暗自).{5,100}",
        r"(他|她)(的)?(心里|内心).{5,100}",
        r""[^""]{5,100}""(他|她)(心想|暗想)"
    ]
    
    segments = []
    
    FOR pattern IN inner_patterns:
        matches = FIND_ALL_WITH_CONTEXT(text, pattern)
        FOR match IN matches:
            segments.APPEND({
                "content": match.group(0),
                "full_text": match.group(0),
                "start_pos": match.start(),
                "end_pos": match.end(),
                "importance": 5  # 默认重要性
            })
        END FOR
    END FOR
    
    RETURN segments
END FUNCTION

# ==================== 辅助判断函数 ====================

FUNCTION ARE_SEMANTICALLY_SIMILAR(sent1, sent2, sent3):
    """判断3句话是否语义相似"""
    # 提取关键词
    keywords1 = EXTRACT_KEYWORDS(sent1)
    keywords2 = EXTRACT_KEYWORDS(sent2)
    keywords3 = EXTRACT_KEYWORDS(sent3)
    
    # 计算重叠度
    overlap_12 = LENGTH(INTERSECT(keywords1, keywords2)) / MAX(LENGTH(UNION(keywords1, keywords2)), 1)
    overlap_23 = LENGTH(INTERSECT(keywords2, keywords3)) / MAX(LENGTH(UNION(keywords2, keywords3)), 1)
    
    # 如果两两重叠度都>60%，认为语义相似
    RETURN overlap_12 > 0.6 AND overlap_23 > 0.6
END FUNCTION

FUNCTION CONTAINS_ACTION_TRIGGER(text):
    """检测是否包含动作触发词"""
    action_triggers = ["突然", "这时", "就在", "只见", "忽然", "猛地", "瞬间", "霎时"]
    RETURN ANY(trigger IN text FOR trigger IN action_triggers)
END FUNCTION

FUNCTION IS_STANDALONE_SENTENCE(text, match):
    """判断匹配是否是独立句子"""
    start = match.start_pos
    end = match.end_pos
    
    # 检查前后是否有句号或换行
    before_char = text[start-1] IF start > 0 ELSE "\n"
    after_char = text[end] IF end < LENGTH(text) ELSE "\n"
    
    RETURN before_char IN ["。", "\n", "！", "？"] AND after_char IN ["。", "\n", "！", "？"]
END FUNCTION

FUNCTION HAS_CONFLICT(text):
    """检测文本是否包含冲突"""
    conflict_keywords = [
        "反驳", "拒绝", "质疑", "对抗", "冲突",
        "不满", "愤怒", "争执", "对峙", "较量"
    ]
    
    RETURN ANY(keyword IN text FOR keyword IN conflict_keywords)
END FUNCTION

FUNCTION CONTAINS_CHARACTER_INTRO(text, parsed_data):
    """检测是否包含角色介绍"""
    IF "characters" NOT IN parsed_data:
        RETURN FALSE
    END IF
    
    FOR char_name, char_info IN parsed_data.characters.ITEMS():
        # 如果文本中包含角色名字 + 特征描述，认为是介绍
        IF char_name IN text AND "特征" IN char_info:
            FOR trait IN char_info.特征:
                IF trait IN text:
                    RETURN TRUE
                END IF
            END IF
        END IF
    END FOR
    
    RETURN FALSE
END FUNCTION

# ==================== 工具函数 ====================

FUNCTION HAS_NESTED_FIELD(obj, field_path):
    """检查嵌套字段是否存在"""
    parts = SPLIT(field_path, ".")
    current = obj
    
    FOR part IN parts:
        IF part NOT IN current:
            RETURN FALSE
        END IF
        current = current[part]
    END FOR
    
    RETURN TRUE
END FUNCTION

FUNCTION FIND_TASK_BY_TYPE(tasks, task_type):
    """在任务列表中查找指定类型的任务"""
    FOR task IN tasks:
        IF task.GET("type", "") == task_type:
            RETURN task
        END IF
    END FOR
    
    RETURN NULL
END FUNCTION

FUNCTION DISTRIBUTE_EVENLY(items, count):
    """将count个数量均匀分配到items中"""
    IF count >= LENGTH(items):
        RETURN items
    END IF
    
    step = LENGTH(items) / count
    result = []
    
    FOR i = 0 TO count-1:
        idx = ROUND(i * step)
        result.APPEND(items[idx])
    END FOR
    
    RETURN result
END FUNCTION

FUNCTION CLAMP(value, min_val, max_val):
    """将值约束在范围内"""
    IF value < min_val:
        RETURN min_val
    ELSE IF value > max_val:
        RETURN max_val
    ELSE:
        RETURN value
    END IF
END FUNCTION

FUNCTION CLEAN_WHITESPACE(text):
    """清理多余空白"""
    # 删除多余空格
    text = REPLACE_PATTERN(text, r" +", " ")
    
    # 删除多余换行（超过2个连续换行）
    text = REPLACE_PATTERN(text, r"\n{3,}", "\n\n")
    
    # 删除行首行尾空格
    text = STRIP(text)
    
    RETURN text
END FUNCTION

# ==================== 统计函数 ====================

FUNCTION COUNT_PARAGRAPHS(text):
    """统计段落数"""
    paragraphs = SPLIT_PARAGRAPHS(text)
    RETURN LENGTH(FILTER(paragraphs, lambda p: LENGTH(p) > 0))
END FUNCTION

FUNCTION AVG_PARAGRAPH_LENGTH(text):
    """计算平均段落长度"""
    paragraphs = SPLIT_PARAGRAPHS(text)
    paragraphs = FILTER(paragraphs, lambda p: LENGTH(p) > 0)
    
    IF LENGTH(paragraphs) == 0:
        RETURN 0
    END IF
    
    total_length = SUM(MAP(paragraphs, LENGTH))
    RETURN total_length / LENGTH(paragraphs)
END FUNCTION

FUNCTION SPLIT_PARAGRAPHS(text):
    """按空行分割段落"""
    RETURN SPLIT(text, "\n\n")
END FUNCTION

FUNCTION SPLIT_SENTENCES(text):
    """按句号分割句子"""
    # 按句号、问号、感叹号分割
    RETURN SPLIT_PATTERN(text, r"[。！？]")
END FUNCTION
```

---

## 【模块11】ERROR_HANDLING - 异常处理（新增）

```python
# ==================== 异常定义 ====================

CLASS CapsuleParseError(Exception):
    """胶囊解析异常"""
    PASS
END CLASS

CLASS SceneWriteError(Exception):
    """场景写作异常"""
    PASS
END CLASS

CLASS PolishError(Exception):
    """润色异常"""
    PASS
END CLASS

CLASS RewriteError(Exception):
    """重写异常"""
    PASS
END CLASS

# ==================== 异常处理函数 ====================

FUNCTION ERROR_REPORT(error):
    """生成错误报告"""
    report = {
        "status": "ERROR",
        "error_type": TYPE(error),
        "error_message": error.message,
        "suggestion": GET_ERROR_SUGGESTION(error)
    }
    
    RETURN report
END FUNCTION

FUNCTION GET_ERROR_SUGGESTION(error):
    """根据错误类型给出建议"""
    IF ISINSTANCE(error, CapsuleParseError):
        RETURN "请检查信息胶囊格式是否正确，确保包含必要的§1-§4模块"
    ELSE IF ISINSTANCE(error, SceneWriteError):
        RETURN "场景写作失败，请检查parsed_data是否完整"
    ELSE IF ISINSTANCE(error, PolishError):
        RETURN "润色失败，但可继续使用未润色版本"
    ELSE IF ISINSTANCE(error, RewriteError):
        RETURN "重写失败，请检查修正指令是否合理"
    ELSE:
        RETURN "未知错误，请联系开发者"
    END IF
END FUNCTION

FUNCTION WRITE_FALLBACK_SCENE(scene_idx, parsed_data):
    """写作失败时的降级方案"""
    fallback_text = f"""
[场景{scene_idx}写作失败，使用降级方案]

{parsed_data.characters.protagonist.name}继续前行。

"""
    
    PRINT "[FALLBACK] 场景{scene_idx}使用降级方案"
    
    RETURN fallback_text
END FUNCTION
```

---

## 【执行示例】

```python
# ==================== 示例执行流程 ====================

FUNCTION MAIN():
    """主程序入口"""
    
    # 1. 读取胶囊
    PRINT "==================== Claude写作系统 v3.1 ====================\n"
    
    TRY:
        CAPSULE = READ_FILE("Capsule.md")
        PRINT "[SYSTEM] 信息胶囊加载成功\n"
    CATCH FileNotFoundError:
        PRINT "[ERROR] 未找到Capsule.md文件"
        RETURN
    END TRY
    
    # 2. 执行主流程
    TRY:
        output = MAIN_EXECUTION(CAPSULE)
        
        # 3. 输出结果
        PRINT "\n==================== 执行完成 ====================\n"
        
        # 3.1 快速摘要
        PRINT output.summary
        
        # 3.2 章节正文
        PRINT "\n==================== 章节正文 ====================\n"
        PRINT output.chapter_content
        
        # 3.3 新增Fact
        PRINT "\n==================== 新增Fact ====================\n"
        PRINT output.new_facts
        
        # 3.4 详细诊断
        PRINT "\n"
        PRINT output.details
        
    CATCH CapsuleParseError AS e:
        PRINT "[ERROR] 胶囊解析失败: {e.message}"
        PRINT "[SUGGESTION] {GET_ERROR_SUGGESTION(e)}"
        
    CATCH SceneWriteError AS e:
        PRINT "[ERROR] 场景写作失败: {e.message}"
        PRINT "[SUGGESTION] {GET_ERROR_SUGGESTION(e)}"
        
    CATCH Exception AS e:
        PRINT "[ERROR] 未知错误: {e.message}"
        PRINT "[SUGGESTION] 请检查输入数据并重试"
    END TRY
    
    PRINT "\n==================== 系统结束 ====================\n"
END FUNCTION

# 执行主程序
IF __name__ == "__main__":
    MAIN()
END IF
```

---

## 【附录】快速参考

### 核心常量
- `WORD_COUNT_TARGET`: 4000字
- `SCENE_WORD_AVG`: 800字/场景
- `TOMATO_CORE_RULES.dialogue_ratio`: [0.35, 0.45]
- `TOMATO_CORE_RULES.inner_monologue_max`: 0.15
- `MAX_REWRITE_ATTEMPTS`: 2次

### 关键流程
1. **解析胶囊** → **生成任务清单** → **分场景写作** → **实时检查** → **集中润色** → **全局诊断** → **问题分析** → **修正重写** → **交付输出**

### 语法规范
- 变量存在检查: `"key" IN dict`
- 默认值获取: `dict.GET(key, default)`
- 数组过滤: `FILTER(array, condition)`

### 病症权重
- 高权重(1.5): D3内心戏、D11密度、D18情绪词、D20视角
- 中权重(1.0-1.2): 大部分病症
- 低权重(0.5): D7逻辑（因果词高频）
- 零权重(0.0): D6/D9/D10（写作阶段处理）

---

**END OF SOP v3.1**
		
