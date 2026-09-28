---
name: annotate-avatar-in-red
description: 将用户上传的头像生成「书页上的头像＋红笔手写箭头＋把各部位叫作食物」风格图片。Use when a user uploads an avatar and asks for 红笔批注头像、食物批注、文献旁批、书页头像梗图, or a new image matching the bundled examples. Supports cartoon, animal, game-character, and real-person avatars.
---

# 头像红笔批注

## 素材与目标

将上传的**一张头像**当作画面的主角，直接生成一张完整图片。**核心笑点是红笔箭头把头像的局部认成各种食物或菜名**，如箭头指向手，旁边写「卤猪蹄」；不是普通的性格分析、夸赞或学术吐槽。此技能自带三张风格参考图：

- `assets/reference-01.jpg`：英文哲学书页、方形卡通头像、给人物局部标上食物名称的红笔手写箭头。
- `assets/reference-02.jpg`：英文书页、粉色卡通形象、贴近轮廓的食物名与红色箭头。
- `assets/reference-03.jpg`：中文论文/电子书页、较大的紫色卡通形象、把肢体和轮廓认成食物的自由批注。参考其**白页内的效果**；不要复制截图的深色工具栏和界面。

如果参考文件只有本地路径，先用 `view_image` 检视，再用 `image_gen` 的 `referenced_image_paths` 纳入生成。若头像仅存在于对话图片，使用 `num_last_images_to_include` 包含头像；不能同时传入该参数和 `referenced_image_paths`。此时用下文的文字风格规格生成即可；参考图作为视觉判断依据，无须将全部图传给生成工具。生成或编辑图片始终调用 `image_gen`，不要用脚本拼接完成作品。

## 一键流程

1. 判断哪个输入是**本次用户头像**，哪个是风格参考。若用户没附头像且当前对话里找不到，才请其上传。默认一张图、竖版书页；若头像适合横向排版，可选择更接近参考图 01、02 的横版局部书页。
2. 观察头像中的可见事实：主体种类、轮廓、姿态、表情、衣服或配饰、颜色、显著细节。尊重用户指定的文字、语气、背景或布局。保持原头像的识别特征；真人头像以原始相貌和表情为准，不擅自替换为某个动漫角色。
3. 先拟定 **4–6 条简短中文食物批注**。每一条箭头文字都必须是**可吃的东西：菜名、零食、食材或带做法的食物名**，不要写抽象评语、人物分析或无关的句子。逐一根据可见部位的形状、颜色、质地做有趣甚至略荒诞的联想：可见的手→「卤猪蹄」，圆脸→「糯米团子」，红脸颊→「蜜桃大福」，黑色卷发→「海苔卷」，蓝眼睛→「蓝莓果冻」。这些只是示范，实际按头像选词，避免每次机械套用；看不见的部位不能强行标注。主体**保持原样**，只用文字把部位戏称为食物，不把人或角色真的改造成食物。不要从头像推断真实身份、性格、疾病等；避免恶意羞辱性称呼。若用户提供确切批注句子，优先逐字使用。
4. 以头像为**编辑目标**、参考图为**风格参考**调用 `image_gen`，生成完整画面。角色约占画面中央偏下的 35%–55%，可像贴纸、照片剪贴或原图贴在书页上；对原头像的风格做最低限度调整。页面有白色或暖白纸张、少量可见的学术书页/论文正文（中英文依用户语境自然选一种）、留白、自然的扫描/阅读质感。内容可为原创、无须完全可读的学术段落，不引用或复刻参考图的原文。围绕人物安排细红箭头/手绘引线、鲜亮红墨水写下**食物名称**，留出较多空白，像读书时突然给人物各处乱起菜名；笔迹宽度、倾斜度稍有变化。文字不要用统一的工整印刷字体。
5. 在提示词中**逐条写出要出现的精确食物名称**，说明每个箭头所指的可见细节；在生成前逐条核对，除用户指定的例外，每一条都应当能回答「这是什么吃的」。禁止照抄参考图角色、文字、页码和截图 UI；禁止添加多余人物、logo、二维码、水印或无关装饰。
6. 查看成品：确认头像仍可认、箭头确实指向相应部位、**所有箭头批注都是食物而不是泛泛的夸赞或学术语句**，整体像一页被红笔涂写的书，而非正式海报或角色设定卡。重点检查汉字是否错写或乱码；若错漏明显，针对相关文字和区域再调用一次 `image_gen` 修正。若多次仍有错字，尽量改成更短、更容易识别的食物名，不要声称难以辨认的文字完全准确。
7. 将生成图直接展示给用户。若用户要求继续修改，按其反馈局部调整并维持头像身份与书页手写风格。

## 可复用提示词骨架

> Use the uploaded avatar as the **only subject / edit target**. Preserve its identity, recognizable silhouette, colors, clothing, facial details and expression. The bundled references define **composition and medium only**, not characters or text to copy. Create a single candid photographed/scanned book page: off-white paper, a few cropped lines of original academic prose at the top or behind the subject; the avatar pasted centrally slightly below midpage, occupying about 45% of the width. The joke is that a reader has identified every visible feature as a FOOD: write only 4–6 short Chinese dish/snack/ingredient names in vivid, messy red handwriting in the margins, each connected with a thin imperfect pen arrow to the corresponding feature. For example, if a hand is visible, the arrow may label it “卤猪蹄”. Exact handwritten food labels, verbatim, with arrow targets: [逐条列出食物名及箭头所指]. Do not use generic compliments, personality comments, academic slogans, or non-food labels. Do not turn the subject's actual anatomy into food; only the annotations make this comparison. Spontaneous, slightly absurd student marginalia; ample blank paper; varied pen pressure. The avatar must remain prominent and recognizable. No app chrome, no copied reference characters, no extra figures, no polished infographic, no printed red text, no watermark. Output one finished image.

如果用户只说「用这个技能」且提供了头像，直接按默认规则生成，不为主题或文案另行提问。
