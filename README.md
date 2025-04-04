# segrs

遥感（RS）图像分割与合并工具，支持大图像的切片和切片的合并操作。

```bash
pip install segrs
```

## 更新情况：

* `0.1.3`fix bug 
* `0.1.0` 初始化

## 核心功能

### 1. `crop_tif` - 图像切片

将大尺寸遥感图像切割为指定大小的小图块，支持重叠区域和命名规则。

#### 参数说明

| 参数名         | 类型      | 默认值       | 说明                                                        |
| -------------- | --------- | ------------ | ----------------------------------------------------------- |
| `input_path` | `str`   | -            | 输入的原始遥感图像路径（支持 `.tif` 格式）                |
| `output_dir` | `str`   | `"output"` | 切片保存的输出目录                                          |
| `tile_size`  | `int`   | `512`      | 每个切片的宽高（单位：像素）                                |
| `overlap`    | `float` | `0`        | 切片之间的重叠比例（如 `0.1` 表示 10% 重叠）              |
| `keep_size`  | `bool`  | `False`    | 是否仅保留与 `tile_size` 完全匹配的切片（边缘不足时跳过） |
| `name`       | `str`   | `"tile"`   | 切片文件名前缀（文件名格式：`{name}_{列}_{行}.tif`）      |

#### 使用示例

<pre><div class="answer-code-wrap"><div class="answer-code-wrap-header"><div class="answer-code-wrap-header-left">python</div><div class="answer-code-wrap-header-right"><span class="ai-button noBg false selected dark undefined"><span role="img" class="anticon yunxiao-icon undefined"><svg width="1em" height="1em" fill="currentColor" aria-hidden="true" focusable="false" class=""><use xlink:href="#yunxiao-insert-line1"></use></svg></span></span><span class="ai-button noBg false selected dark undefined"><span role="img" class="anticon yunxiao-icon undefined"><svg width="1em" height="1em" fill="currentColor" aria-hidden="true" focusable="false" class=""><use xlink:href="#yunxiao-copy-line"></use></svg></span></span><span class="ai-button noBg false selected dark undefined"><span role="img" class="anticon yunxiao-icon undefined"><svg width="1em" height="1em" fill="currentColor" aria-hidden="true" focusable="false" class=""><use xlink:href="#yunxiao-additive-code-file-line"></use></svg></span></span></div></div><div node="[object Object]" class="answer-code-wrap-body" requestid="f1aa7ad4-615e-40fb-873c-03320c01b57c" tasktype="FREE_INPUT"><code class="language-python"><span class="token">from</span><span> segrs</span><span class="token">.</span><span>RSsplit </span><span class="token">import</span><span> crop_tif
</span>
<span></span><span class="token"># 将 input.tif 切割为 512x512 像素的小图，重叠 10%，输出到 output_dir 目录</span><span>
</span><span>crop_tif</span><span class="token">(</span><span>
</span><span>    input_path</span><span class="token">=</span><span class="token">"input.tif"</span><span class="token">,</span><span>
</span><span>    output_dir</span><span class="token">=</span><span class="token">"output_dir"</span><span class="token">,</span><span>
</span><span>    tile_size</span><span class="token">=</span><span class="token">512</span><span class="token">,</span><span>
</span><span>    overlap</span><span class="token">=</span><span class="token">0.1</span><span class="token">,</span><span>
</span><span>    keep_size</span><span class="token">=</span><span class="token">True</span><span class="token">,</span><span>
</span><span>    name</span><span class="token">=</span><span class="token">"rs_slice"</span><span>
</span><span></span><span class="token">)</span></code></div></div></pre>

---

### 2. `merge_tiles` - 图像合并

将切割后的切片重新合并为原始大图像。

#### 参数说明

| 参数名          | 类型      | 默认值     | 说明                                                            |
| --------------- | --------- | ---------- | --------------------------------------------------------------- |
| `input_dir`   | `str`   | -          | 包含切片文件的目录（文件名需包含行列编号，如 `tile_1_2.tif`） |
| `output_path` | `str`   | -          | 合并后的输出文件路径                                            |
| `tile_size`   | `int`   | `512`    | 切片的原始尺寸（需与切割时一致）                                |
| `overlap`     | `float` | `0`      | 切片的重叠比例（需与切割时一致）                                |
| `name`        | `str`   | `"tile"` | 切片文件名前缀（需与切割时一致）                                |

#### 使用示例

<pre><div class="answer-code-wrap"><div class="answer-code-wrap-header"><div class="answer-code-wrap-header-left">python</div><div class="answer-code-wrap-header-right"><span class="ai-button noBg false selected dark undefined"><span role="img" class="anticon yunxiao-icon undefined"><svg width="1em" height="1em" fill="currentColor" aria-hidden="true" focusable="false" class=""><use xlink:href="#yunxiao-insert-line1"></use></svg></span></span><span class="ai-button noBg false selected dark undefined"><span role="img" class="anticon yunxiao-icon undefined"><svg width="1em" height="1em" fill="currentColor" aria-hidden="true" focusable="false" class=""><use xlink:href="#yunxiao-copy-line"></use></svg></span></span><span class="ai-button noBg false selected dark undefined"><span role="img" class="anticon yunxiao-icon undefined"><svg width="1em" height="1em" fill="currentColor" aria-hidden="true" focusable="false" class=""><use xlink:href="#yunxiao-additive-code-file-line"></use></svg></span></span></div></div><div node="[object Object]" class="answer-code-wrap-body" requestid="f1aa7ad4-615e-40fb-873c-03320c01b57c" tasktype="FREE_INPUT"><code class="language-python"><span class="token">from</span><span> segrs</span><span class="token">.</span><span>RSmerge </span><span class="token">import</span><span> merge_tiles
</span>
<span></span><span class="token"># 合并 input_dir 目录下的切片文件，生成 merged.tif</span><span>
</span><span>merge_tiles</span><span class="token">(</span><span>
</span><span>    input_dir</span><span class="token">=</span><span class="token">"input_dir"</span><span class="token">,</span><span>
</span><span>    output_path</span><span class="token">=</span><span class="token">"merged.tif"</span><span class="token">,</span><span>
</span><span>    tile_size</span><span class="token">=</span><span class="token">512</span><span class="token">,</span><span>
</span><span>    overlap</span><span class="token">=</span><span class="token">0.1</span><span class="token">,</span><span>
</span><span>    name</span><span class="token">=</span><span class="token">"rs_slice"</span><span>
</span><span></span><span class="token">)</span></code></div></div></pre>

## 依赖项

确保安装以下依赖：

<pre><div class="answer-code-wrap"><div class="answer-code-wrap-header"><div class="answer-code-wrap-header-left">bash</div><div class="answer-code-wrap-header-right"><span class="ai-button noBg false selected dark undefined"><span role="img" class="anticon yunxiao-icon undefined"><svg width="1em" height="1em" fill="currentColor" aria-hidden="true" focusable="false" class=""><use xlink:href="#yunxiao-insert-line1"></use></svg></span></span><span class="ai-button noBg false selected dark undefined"><span role="img" class="anticon yunxiao-icon undefined"><svg width="1em" height="1em" fill="currentColor" aria-hidden="true" focusable="false" class=""><use xlink:href="#yunxiao-copy-line"></use></svg></span></span><span class="ai-button noBg false selected dark undefined"><span role="img" class="anticon yunxiao-icon undefined"><svg width="1em" height="1em" fill="currentColor" aria-hidden="true" focusable="false" class=""><use xlink:href="#yunxiao-additive-code-file-line"></use></svg></span></span></div></div><div node="[object Object]" class="answer-code-wrap-body" requestid="f1aa7ad4-615e-40fb-873c-03320c01b57c" tasktype="FREE_INPUT"><code class="language-bash"><span>pip </span><span class="token">install</span><span> rasterio numpy tqdm</span></code></div></div></pre>

## 注意事项

1. 输入图像需为 `.tif` 格式（通过 `rasterio` 库支持）
2. 切片文件名需严格遵循 `name_{col}_{row}.tif` 格式（如 `tile_3_5.tif`）
3. 合并时需保证 `tile_size` 和 `overlap` 参数与切割时一致
