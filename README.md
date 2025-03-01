# **설치 방법** 

# Conda 환경에서 pdf2zh 설치 가이드

## 개요
이 문서는 [pdf2zh](https://github.com/Byaidu/PDFMathTranslate)를 Conda 가상환경에서 올바르게 설치하고 실행하는 방법을 설명합니다.  
특히 `pdf2zh` 명령어가 정상적으로 인식되지 않는 문제를 방지하기 위해, **가상환경 내부에만 설치하는 방법**을 다룹니다.

## 1. Conda 가상환경 생성 및 활성화
먼저, 새로운 Conda 가상환경(예: `pdf2zh`)을 만들고 활성화합니다.
```bash
conda create -n pdf2zh python=3.10
conda activate pdf2zh
```
> 필요한 경우 Python 버전(3.10 이상)을 조정할 수 있습니다.

## 2. 기존 pdf2zh 제거 (필수)
pdf2zh가 **사용자 전역 경로**(user-level site-packages)에 설치되었을 경우, CLI 명령어가 인식되지 않을 수 있습니다.  
아래 명령어를 실행하여 기존 설치를 제거하세요.
```bash
pip uninstall pdf2zh
```

## 3. pdf2zh 가상환경 내부에 설치하기
**반드시** `--no-user` 옵션을 사용하여 가상환경 내부에만 설치하세요.
```bash
pip install pdf2zh --no-user
```
이렇게 하면 pdf2zh가 Conda 가상환경의 `site-packages`에 올바르게 설치되며, CLI 명령어가 정상 작동합니다.

## 4. 설치 확인
설치 후, 아래 명령어로 `pdf2zh`가 인식되는지 확인합니다.
```powershell
Get-Command pdf2zh
```
출력 결과에 `C:\Users\<사용자이름>\.conda\envs\pdf2zh\Scripts\pdf2zh.exe` 또는 유사한 경로가 포함되어 있어야 합니다.

## 5. pdf2zh 사용법
### 5.1 CLI 모드 (명령어 실행)
터미널에서 PDF 번역을 실행하려면 아래 명령어를 사용합니다.
```bash
pdf2zh path\to\document.pdf
```
변환된 PDF 파일 두 개(번역본, 원본-번역 비교본)가 현재 작업 폴더에 생성됩니다.

### 5.2 GUI 모드 (웹 인터페이스)
인터랙티브 웹 UI를 실행하려면 다음 명령어를 사용합니다.
```bash
pdf2zh -i
```
웹 브라우저가 자동으로 실행되지 않으면, 수동으로 아래 주소에 접속하세요.
```
http://localhost:7860
```

## 6. 문제 해결 (Troubleshooting)
### 6.1 `pdf2zh` 명령어가 인식되지 않는 경우
- 올바른 Conda 가상환경이 활성화되어 있는지 확인하세요.
  ```bash
  conda activate pdf2zh
  ```
- `Scripts` 폴더에 `pdf2zh.exe`가 존재하는지 확인하세요.
  ```powershell
  dir $env:CONDA_PREFIX\Scripts\pdf2zh*
  ```
- 만약 `pdf2zh` 실행 파일이 가상환경이 아닌 **사용자 폴더**(`AppData\Roaming\Python\Python310\Scripts` 등)에 있다면,  
  **1번 과정(기존 설치 제거)**을 수행한 후 다시 설치하세요.

### 6.2 `argos-translate` 관련 경고 메시지
실행 시 `"argos-translate is not installed"` 경고가 표시될 수 있습니다.  
이는 pdf2zh의 특정 번역 엔진을 사용할 때만 필요한 라이브러리이므로, 무시해도 됩니다.  
해당 기능이 필요하면 아래 명령어로 추가 설치하세요.
```bash
pip install argostranslate
```

### 6.3 추가 번역 서비스 사용
DeepL, OpenAI 등의 번역 API를 사용하려면 추가적인 환경 변수가 필요할 수 있습니다.  
자세한 내용은 [공식 문서](https://github.com/Byaidu/PDFMathTranslate/blob/main/docs/ADVANCED.md#services)를 참고하세요.

# **아나콘다 가상환경에서 코드 실행** 
``` text
# 1. pdf2zh가 설치된 conda 환경 활성화
conda activate pdf2zh

# 2. API 관련 환경변수 설정
## Google Gemini API
$env:GEMINI_MODEL = "gemini-2.0-flash"
$env:GEMINI_API_KEY = "AIzaSyBrO4IdklakfJxnqioyIWRwELucohZjqdo"  

## DeepSeek API 설정
$env:DEEPSEEK_MODEL = "deepseek-chat"
$env:DEEPSEEK_API_KEY = "sk-7319a3417d9a4b5da5f5d02d2fae0338" 

# 3. 번역할 언어 설정 (원본: 영어, 대상: 한국어)
$env:SOURCE_LANG = "en"
$env:TARGET_LANG = "ko"

# 4. 입력 폴더와 출력 폴더 지정
$inputDir = "C:\Users\alice\Documents\PAPERS\TRANSLATE\waiting"
$outputDir = "C:\Users\alice\Documents\PAPERS\TRANSLATE\translated"

# 5. custom_prompt.txt 파일 경로 설정
$env:PDF2ZH_PROMPT = "C:\Users\alice\Documents\PAPERS\TRANSLATE\custom_prompt.txt"

# 6. 멀티스레드 설정
$env:THREAD = 5

# 7. PDF 번역 실행 (옵션들을 순서대로 지정)
#
# - `--dir $inputDir`       : 번역할 PDF 파일들이 위치한 입력 폴더를 지정합니다.
# - `-li $env:SOURCE_LANG`  : 원본 언어를 지정합니다.
# - `-lo $env:TARGET_LANG`  : 대상 언어를 지정합니다.
# - `--prompt $env:PDF2ZH_PROMPT` : 사용자가 지정한 custom prompt 파일의 경로를 설정합니다.
# - `-s gemini`             : 번역 서비스를 선택합니다.
#     예시:
#       - Gemini API 사용 시: `-s gemini`
#       - DeepSeek API 사용 시: `-s deepseek`
#     각 서비스에 맞는 환경변수를 반드시 설정해야 합니다.
# - `-t $env:THREAD`        : 멀티스레드 개수를 지정합니다.
# - `-o $outputDir`         : 번역 결과가 저장될 출력 폴더를 지정합니다.
#--------------------------------------------------------------------------
pdf2zh --dir $inputDir `
  -li $env:SOURCE_LANG `
  -lo $env:TARGET_LANG `
  --prompt $env:PDF2ZH_PROMPT `
  -s deepseek `
  -t $env:THREAD `
  -o $outputDir
```

# **PROMPT** 

영한문혼용체, 레퍼런스 부분 번역하지 말것 

``` text
You are a technical translator specializing in 과학/공학 분야 문서. Maintain original terminology accuracy while applying 영한문혼용체 (Korean sentence structure + English technical terms/numbers).

Translate the following text to ${lang_out} in a formal tone. 
1. 전문 용어(예: MPC, RMSE), 모델/알고리즘 이름(예: PID controller), 수학적 표현(예: 6-DOF)은 **영어 원문 보존**  
2. 숫자/단위(예: 41%, 58% 감소)는 영어 형식(%, Hz, mm)으로 유지  
3. 문장 구조는 한국어 문법에 완전히 종속  
4. **굵게 표시** 필요 시 용어에만 적용 (예: **adaptive MPC**)  

[Special Instructions - DO NOT TRANSLATE]  
1. **References/Bibliography Sections**  
   - Identify sections titled *References*, *Bibliography*, *참고문헌*, or equivalents.  
   - **Leave these sections fully untranslated** (titles, author names, publication years, journal names, formatting).  

2. **Inline Citations**  
   - Preserve all inline citations **exactly as-is**, including:  
     - Parenthetical citations: *"(Author et al., 2023)"*  
     - Bracketed citations: *"[1]"*  
     - Author-year formats: *"Author (Year)"*  
     - Footnotes/superscripts: *"See footnote¹"*  

3. **General Guidelines**  
   - Translate **only** the main body (abstract, introduction, results, discussion).  
   - **Preservation criteria**:  
     - Numbers in brackets: [1] → [1] (no change)  
     - Italicized journal names: *Journal of X* → *Journal of X*  
     - Original punctuation/spacing: e.g., commas after years → "(Lee et al., 2023, p.12)"  
   - **Default action**: If citation/reference detection is ambiguous → leave untranslated.  

[Example]  
**Original**:  
"This result aligns with prior research (Choi et al., 2022). References: [1] Kim et al., 2021. *Journal of Science*."  

**Correct Translation**:  
"본 결과는 선행 연구(Choi et al., 2022)와 일치합니다. References: [1] Kim et al., 2021. *Journal of Science*."  


아래 예시는 영한문혼용체로 번역하라는 지시에 매우 잘 부합하는 예시이니 이를 준수하도록 해라. 

1. Computational Paper 예시  
"본 연구에서는 transformer architecture 기반의 neural network가 long-range dependencies 학습 시 기존 RNN 대비 72% 향상된 attention mechanism 효율성을 입증하였으며, 특히 GPU parallelization 환경에서 latency가 33% 감소함을 확인"

2. Motor Control Paper 예시  
**"**실험 결과 6-DOF robotic manipulator의 trajectory tracking 성능이 PID controller 대비 adaptive MPC 적용 시 root mean square error(RMSE)가 41% 개선되었으며, actuator saturation 발생 빈도도 58% 감소함" ← ★추천★


Source Text: ${text}

Translated Text:
```
















<div align="center">

English | [简体中文](docs/README_zh-CN.md) | [繁體中文](docs/README_zh-TW.md) | [日本語](docs/README_ja-JP.md) | [한국어](docs/README_ko-KR.md)

<img src="./docs/images/banner.png" width="320px"  alt="PDF2ZH"/>

<h2 id="title">PDFMathTranslate</h2>

<p>
  <!-- PyPI -->
  <a href="https://pypi.org/project/pdf2zh/">
    <img src="https://img.shields.io/pypi/v/pdf2zh"></a>
  <a href="https://pepy.tech/projects/pdf2zh">
    <img src="https://static.pepy.tech/badge/pdf2zh"></a>
  <a href="https://hub.docker.com/repository/docker/byaidu/pdf2zh">
    <img src="https://img.shields.io/docker/pulls/byaidu/pdf2zh"></a>
  <a href="https://gitcode.com/Byaidu/PDFMathTranslate/overview">
    <img src="https://gitcode.com/Byaidu/PDFMathTranslate/star/badge.svg"></a>
  <a href="https://huggingface.co/spaces/reycn/PDFMathTranslate-Docker">
    <img src="https://img.shields.io/badge/%F0%9F%A4%97-Online%20Demo-FF9E0D"></a>
  <a href="https://www.modelscope.cn/studios/AI-ModelScope/PDFMathTranslate">
    <img src="https://img.shields.io/badge/ModelScope-Demo-blue"></a>
  <a href="https://github.com/Byaidu/PDFMathTranslate/pulls">
    <img src="https://img.shields.io/badge/contributions-welcome-green"></a>
  <a href="https://t.me/+Z9_SgnxmsmA5NzBl">
    <img src="https://img.shields.io/badge/Telegram-2CA5E0?style=flat-squeare&logo=telegram&logoColor=white"></a>
  <!-- License -->
  <a href="./LICENSE">
    <img src="https://img.shields.io/github/license/Byaidu/PDFMathTranslate"></a>
</p>

<a href="https://trendshift.io/repositories/12424" target="_blank"><img src="https://trendshift.io/api/badge/repositories/12424" alt="Byaidu%2FPDFMathTranslate | Trendshift" style="width: 250px; height: 55px;" width="250" height="55"/></a>

</div>

PDF scientific paper translation and bilingual comparison.

- 📊 Preserve formulas, charts, table of contents, and annotations _([preview](#preview))_.
- 🌐 Support [multiple languages](#language), and diverse [translation services](#services).
- 🤖 Provides [commandline tool](#usage), [interactive user interface](#gui), and [Docker](#docker)

Feel free to provide feedback in [GitHub Issues](https://github.com/Byaidu/PDFMathTranslate/issues) or [Telegram Group](https://t.me/+Z9_SgnxmsmA5NzBl).

For details on how to contribute, please consult the [Contribution Guide](https://github.com/Byaidu/PDFMathTranslate/wiki/Contribution-Guide---%E8%B4%A1%E7%8C%AE%E6%8C%87%E5%8D%97).

<h2 id="updates">Updates</h2>

- [Feb. 22 2025] Better release CI and well-packaged windows-amd64 exe (by [@awwaawwa](https://github.com/awwaawwa))
- [Dec. 24 2024] The translator now supports local models on [Xinference](https://github.com/xorbitsai/inference) _(by [@imClumsyPanda](https://github.com/imClumsyPanda))_
- [Dec. 19 2024] Non-PDF/A documents are now supported using `-cp` _(by [@reycn](https://github.com/reycn))_
- [Dec. 13 2024] Additional support for backend by _(by [@YadominJinta](https://github.com/YadominJinta))_
- [Dec. 10 2024] The translator now supports OpenAI models on Azure _(by [@yidasanqian](https://github.com/yidasanqian))_

<h2 id="preview">Preview</h2>

<div align="center">
<img src="./docs/images/preview.gif" width="80%"/>
</div>

<h2 id="demo">Online Service 🌟</h2>

You can try our application out using either of the following demos:

- [Public free service](https://pdf2zh.com/) online without installation _(recommended)_.
- [Immersive Translate - BabelDOC](https://app.immersivetranslate.com/babel-doc/) 1000 free pages per month. _(recommended)_
- [Demo hosted on HuggingFace](https://huggingface.co/spaces/reycn/PDFMathTranslate-Docker)
- [Demo hosted on ModelScope](https://www.modelscope.cn/studios/AI-ModelScope/PDFMathTranslate) without installation.

Note that the computing resources of the demo are limited, so please avoid abusing them.

<h2 id="install">Installation and Usage</h2>

### Methods

For different use cases, we provide distinct methods to use our program:

<details open>
  <summary>1. UV install</summary>

1. Python installed (3.10 <= version <= 3.12)
2. Install our package:

   ```bash
   pip install uv
   uv tool install --python 3.12 pdf2zh
   ```

3. Execute translation, files generated in [current working directory](https://chatgpt.com/share/6745ed36-9acc-800e-8a90-59204bd13444):

   ```bash
   pdf2zh document.pdf
   ```

</details>

<details>
  <summary>2. Windows exe</summary>

1. Download pdf2zh-version-win64.zip from [release page](https://github.com/Byaidu/PDFMathTranslate/releases)

2. Unzip and double-click `pdf2zh.exe` to run.

</details>

<details>
  <summary>3. Graphic user interface</summary>
1. Python installed (3.10 <= version <= 3.12)
2. Install our package:

```bash
pip install pdf2zh
```

3. Start using in browser:

   ```bash
   pdf2zh -i
   ```

4. If your browswer has not been started automatically, goto

   ```bash
   http://localhost:7860/
   ```

   <img src="./docs/images/gui.gif" width="500"/>

See [documentation for GUI](./docs/README_GUI.md) for more details.

</details>

<details>
  <summary>4. Docker</summary>

1. Pull and run:

   ```bash
   docker pull byaidu/pdf2zh
   docker run -d -p 7860:7860 byaidu/pdf2zh
   ```

2. Open in browser:

   ```
   http://localhost:7860/
   ```

For docker deployment on cloud service:

<div>
<a href="https://www.heroku.com/deploy?template=https://github.com/Byaidu/PDFMathTranslate">
  <img src="https://www.herokucdn.com/deploy/button.svg" alt="Deploy" height="26"></a>
<a href="https://render.com/deploy">
  <img src="https://render.com/images/deploy-to-render-button.svg" alt="Deploy to Koyeb" height="26"></a>
<a href="https://zeabur.com/templates/5FQIGX?referralCode=reycn">
  <img src="https://zeabur.com/button.svg" alt="Deploy on Zeabur" height="26"></a>
<a href="https://app.koyeb.com/deploy?type=git&builder=buildpack&repository=github.com/Byaidu/PDFMathTranslate&branch=main&name=pdf-math-translate">
  <img src="https://www.koyeb.com/static/images/deploy/button.svg" alt="Deploy to Koyeb" height="26"></a>
</div>

</details>

<details>
  <summary>5. Zotero Plugin</summary>
See [Zotero PDF2zh](https://github.com/guaguastandup/zotero-pdf2zh) for more details.

</details>

<details>
  <summary>6. Commandline</summary>

1. Python installed (3.10 <= version <= 3.12)
2. Install our package:

   ```bash
   pip install pdf2zh
   ```

3. Execute translation, files generated in [current working directory](https://chatgpt.com/share/6745ed36-9acc-800e-8a90-59204bd13444):

   ```bash
   pdf2zh document.pdf
   ```

</details>

> [!TIP]
>
> - If you're using Windows and cannot open the file after downloading, please install [vc_redist.x64.exe](https://aka.ms/vs/17/release/vc_redist.x64.exe) and try again.
>
> - If you cannot access Docker Hub, please try the image on [GitHub Container Registry](https://github.com/Byaidu/PDFMathTranslate/pkgs/container/pdfmathtranslate).
> ```bash
> docker pull ghcr.io/byaidu/pdfmathtranslate
> docker run -d -p 7860:7860 ghcr.io/byaidu/pdfmathtranslate
> ```

### Unable to install?

The present program needs an AI model(`wybxc/DocLayout-YOLO-DocStructBench-onnx`) before working and some users are not able to download due to network issues. If you have a problem with downloading this model, we provide a workaround using the following environment variable:

```shell
set HF_ENDPOINT=https://hf-mirror.com
```

For PowerShell user:

```shell
$env:HF_ENDPOINT = https://hf-mirror.com
```

If the solution does not work to you / you encountered other issues, please refer to [frequently asked questions](https://github.com/Byaidu/PDFMathTranslate/wiki#-faq--%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98).

<h2 id="usage">Advanced Options</h2>

Execute the translation command in the command line to generate the translated document `example-mono.pdf` and the bilingual document `example-dual.pdf` in the current working directory. Use Google as the default translation service. More support translation services can find [HERE](https://github.com/Byaidu/PDFMathTranslate/blob/main/docs/ADVANCED.md#services).

<img src="./docs/images/cmd.explained.png" width="580px"  alt="cmd"/>

In the following table, we list all advanced options for reference:

| Option         | Function                                                                                                      | Example                                        |
| -------------- | ------------------------------------------------------------------------------------------------------------- | ---------------------------------------------- |
| files          | Local files                                                                                                   | `pdf2zh ~/local.pdf`                           |
| links          | Online files                                                                                                  | `pdf2zh http://arxiv.org/paper.pdf`            |
| `-i`           | [Enter GUI](#gui)                                                                                             | `pdf2zh -i`                                    |
| `-p`           | [Partial document translation](https://github.com/Byaidu/PDFMathTranslate/blob/main/docs/ADVANCED.md#partial) | `pdf2zh example.pdf -p 1`                      |
| `-li`          | [Source language](https://github.com/Byaidu/PDFMathTranslate/blob/main/docs/ADVANCED.md#languages)            | `pdf2zh example.pdf -li en`                    |
| `-lo`          | [Target language](https://github.com/Byaidu/PDFMathTranslate/blob/main/docs/ADVANCED.md#languages)            | `pdf2zh example.pdf -lo zh`                    |
| `-s`           | [Translation service](https://github.com/Byaidu/PDFMathTranslate/blob/main/docs/ADVANCED.md#services)         | `pdf2zh example.pdf -s deepl`                  |
| `-t`           | [Multi-threads](https://github.com/Byaidu/PDFMathTranslate/blob/main/docs/ADVANCED.md#threads)                | `pdf2zh example.pdf -t 1`                      |
| `-o`           | Output dir                                                                                                    | `pdf2zh example.pdf -o output`                 |
| `-f`, `-c`     | [Exceptions](https://github.com/Byaidu/PDFMathTranslate/blob/main/docs/ADVANCED.md#exceptions)                | `pdf2zh example.pdf -f "(MS.*)"`               |
| `-cp`          | Compatibility Mode                                                                                            | `pdf2zh example.pdf --compatible`              |
| `--skip-subset-fonts` | [Skip font subset](https://github.com/Byaidu/PDFMathTranslate/blob/main/docs/ADVANCED.md#font-subset)  | `pdf2zh example.pdf --skip-subset-fonts`       |
| `--share`      | Public link                                                                                                   | `pdf2zh -i --share`                            |
| `--authorized` | [Authorization](https://github.com/Byaidu/PDFMathTranslate/blob/main/docs/ADVANCED.md#auth)                   | `pdf2zh -i --authorized users.txt [auth.html]` |
| `--prompt`     | [Custom Prompt](https://github.com/Byaidu/PDFMathTranslate/blob/main/docs/ADVANCED.md#prompt)                 | `pdf2zh --prompt [prompt.txt]`                 |
| `--onnx`       | [Use Custom DocLayout-YOLO ONNX model]                                                                        | `pdf2zh --onnx [onnx/model/path]`              |
| `--serverport` | [Use Custom WebUI port]                                                                                       | `pdf2zh --serverport 7860`                     |
| `--dir`        | [batch translate]                                                                                             | `pdf2zh --dir /path/to/translate/`             |
| `--config`     | [configuration file](https://github.com/Byaidu/PDFMathTranslate/blob/main/docs/ADVANCED.md#cofig)             | `pdf2zh --config /path/to/config/config.json`  |
| `--serverport` | [custom gradio server port]                                                                                   | `pdf2zh --serverport 7860`                     |

For detailed explanations, please refer to our document about [Advanced Usage](./docs/ADVANCED.md) for a full list of each option.

<h2 id="downstream">Secondary Development (APIs)</h2>

For downstream applications, please refer to our document about [API Details](./docs/APIS.md) for futher information about:

- [Python API](./docs/APIS.md#api-python), how to use the program in other Python programs
- [HTTP API](./docs/APIS.md#api-http), how to communicate with a server with the program installed

<h2 id="todo">TODOs</h2>

- [ ] Parse layout with DocLayNet based models, [PaddleX](https://github.com/PaddlePaddle/PaddleX/blob/17cc27ac3842e7880ca4aad92358d3ef8555429a/paddlex/repo_apis/PaddleDetection_api/object_det/official_categories.py#L81), [PaperMage](https://github.com/allenai/papermage/blob/9cd4bb48cbedab45d0f7a455711438f1632abebe/README.md?plain=1#L102), [SAM2](https://github.com/facebookresearch/sam2)

- [ ] Fix page rotation, table of contents, format of lists

- [ ] Fix pixel formula in old papers

- [ ] Async retry except KeyboardInterrupt

- [ ] Knuth–Plass algorithm for western languages

- [ ] Support non-PDF/A files

- [ ] Plugins of [Zotero](https://github.com/zotero/zotero) and [Obsidian](https://github.com/obsidianmd/obsidian-releases)

<h2 id="acknowledgement">Acknowledgements</h2>

- [Immersive Translation](https://immersivetranslate.com) sponsors monthly Pro membership redemption codes for active contributors to this project, see details at: [CONTRIBUTOR_REWARD.md](https://github.com/funstory-ai/BabelDOC/blob/main/docs/CONTRIBUTOR_REWARD.md)

- Document merging: [PyMuPDF](https://github.com/pymupdf/PyMuPDF)

- Document parsing: [Pdfminer.six](https://github.com/pdfminer/pdfminer.six)

- Document extraction: [MinerU](https://github.com/opendatalab/MinerU)

- Document Preview: [Gradio PDF](https://github.com/freddyaboulton/gradio-pdf)

- Multi-threaded translation: [MathTranslate](https://github.com/SUSYUSTC/MathTranslate)

- Layout parsing: [DocLayout-YOLO](https://github.com/opendatalab/DocLayout-YOLO)

- Document standard: [PDF Explained](https://zxyle.github.io/PDF-Explained/), [PDF Cheat Sheets](https://pdfa.org/resource/pdf-cheat-sheets/)

- Multilingual Font: [Go Noto Universal](https://github.com/satbyy/go-noto-universal)

<h2 id="contrib">Contributors</h2>

<a href="https://github.com/Byaidu/PDFMathTranslate/graphs/contributors">
  <img src="https://opencollective.com/PDFMathTranslate/contributors.svg?width=890&button=false" />
</a>

![Alt](https://repobeats.axiom.co/api/embed/dfa7583da5332a11468d686fbd29b92320a6a869.svg "Repobeats analytics image")

<h2 id="star_hist">Star History</h2>

<a href="https://star-history.com/#Byaidu/PDFMathTranslate&Date">
 <picture>
   <source media="(prefers-color-scheme: dark)" srcset="https://api.star-history.com/svg?repos=Byaidu/PDFMathTranslate&type=Date&theme=dark" />
   <source media="(prefers-color-scheme: light)" srcset="https://api.star-history.com/svg?repos=Byaidu/PDFMathTranslate&type=Date" />
   <img alt="Star History Chart" src="https://api.star-history.com/svg?repos=Byaidu/PDFMathTranslate&type=Date"/>
 </picture>
</a>
