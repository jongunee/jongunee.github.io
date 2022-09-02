---
layout: post
title: Helm Chart - 함수 & 파이프라인
description: >
  [참조]: 인프런 - 대세는 쿠버네티스 [Helm편]
sitemap: true
hide_last_modified: true
categories:
  - web
  - docker
---

# Helm Chart - 함수 & 파이프라인

* toc
{:toc .large-only}

## 함수와 파이프라인

**차트 Template 생성**

```cmd
helm create mychart
```

**test-vsalues.yaml**

```yml
func:
  enabled: true
  
pipe:
  log: info
```

**ConfigMap 추가**

```cmd
vim cm1.yaml
```

```yml
apiVersion: v1
kind: ConfigMap
metadata:
  name: function-and-pipeline
data:
  Function_Argument:
    quote: {{ quote .Values.func.enabled }}     # quote(arg1)
    include: {{ include "mychart.name" . }}  # include(arg1, arg2)

  Function_Quote:
    function_case1: {{ .Values.func.enabled }}
    function_case2: {{ quote .Values.func.enabled }}
    function_case3: "{{ .Values.func.enabled }}"

  Pipeline:
    upper: {{ .Values.pipe.log | upper }}
    upper.repeat: {{ .Values.pipe.log | upper | repeat 2 }}
    upper.repeat.quote: {{ .Values.pipe.log | upper | repeat 2 | quote }}
```

**Template 명령어**

```cmd
helm template mychart ./../ -f ./../test-values.yaml
```

**data 출력 결과**

```yml
data:
  Function_Argument:
    quote: "true"     # quote(arg1)
    include: mychart  # include(arg1, arg2)

  Function_Quote:
    function_case1: true
    function_case2: "true"
    function_case3: "true"

  Pipeline:
    upper: INFO
    upper.repeat: INFOINFO
    upper.repeat.quote: "INFOINFO"
```

## 흐름 제어

**test-values.yaml**

```yml
dev:
  env: dev
  log: info

qa:
  env: qa
  log: info
  
prod:
  env: prod
  log: 
  
data:
  - a
  - b
  - c
```

### if

**if문 false 조건**

```yml
Number:0
String: ""
List: []
Object: {}
Boolean: false
Null
```

**if문 함수**

| 함수 | 의미 |
| --- | --- |
| and | && |
| or | \|\| |
| ne | != |
| not | ! |
| eq | = |
| ge | >= |
| gt | > |
| le | <= |
| lt | < |
| default | default |
| empty | empty |

**cm2-2.yaml**

```yml
apiVersion: v1
kind: ConfigMap
metadata:
  name: flow-if
data:
  dev:
    env: {{ .Values.dev.env }}
    {{- if eq .Values.dev.env "dev" }}
    log: debug
    {{- else if .Values.dev.log }}
    log: {{ .Values.dev.log }}
    {{- else }}
    log: error
    {{ end }}

  qa:
    env: {{ .Values.qa.env }}
    {{- if eq .Values.qa.env "dev" }}
    log: debug
    {{- else if .Values.qa.log }}
    log: {{ .Values.qa.log }}
    {{- else }}
    log: error
    {{- end }}

  prod:
    env: {{ .Values.prod.env }}
    {{ if eq .Values.prod.env "dev" }}
    log: debug
    {{ else if .Values.prod.log }}
    log: {{ .Values.prod.log }}
    {{ else }}
    log: error
    {{ end }}
```

**data 출력 결과**

```yml
data:
  dev:
    env: dev
    log: debug

  qa:
    env: qa
    log: info

  prod:
    env: prod

    log: error
```

- prod에서 공백이 생긴 이유는 위 yaml 파일에서 괄호 바로 뒤에 하이픈(-) 입력을 안해줬기 때문

### With

**cm2-3.yaml**

```yml
apiVersion: v1
kind: ConfigMap
metadata:
  name: flow-with
data:
  dev:
  {{- with .Values.dev }}
    env: {{ .env }}
    log: {{ .log }}
  {{- end }}

  qa:
  {{- with .Values.qa }}
    env: {{ .env }}
    log: {{ .log }}
  {{- end }}
  
  prod:
  {{- with .Values.prod }}
    env: {{ .env }}
    log: {{ .log }}
  {{- end }}
```

**data 출력 결과**

```yml
data:
  dev:
    env: dev
    log: info

  qa:
    env: qa
    log: info

  prod:
    env: prod
    log:
```

### Range

**cm2-4.yaml**

```yml
apiVersion: v1
kind: ConfigMap
metadata:
  name: flow-range
data:
  yaml:
  {{- .Values.data | toYaml | nindent 2 }}

  range:
  {{- range .Values.data }}
  - {{ . }}
  {{- end }}

  range-quote:
  {{- range .Values.data }}
  - {{ . | quote }}
  {{- end }}

  range-upper-quote:
  {{- range .Values.data }}
  - {{ . | upper | quote }}
  {{- end }}
```

**data 출력 결과**

```yml
data:
  yaml:
  - a
  - b
  - c

  range:
  - a
  - b
  - c

  range-quote:
  - "a"
  - "b"
  - "c"

  range-upper-quote:
  - "A"
  - "B"
  - "C"
```

## 여러 함수

- helm의 템플릿 파일은 Golang 문법을 따름

**test-values.yaml**

```yml
print:
  a: "1"
  b: "2"

ternary: 
  case1: true
  case2: false
  
default:
  nil: 
  list: []
  object: {}
  number: 0
  string: ""
  boolean: false
```

**cm3.yaml**

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: string-func
data:
  print:
    print:  {{ print "Hard Cording" }}
    printf: {{ printf "%s-%s" .Values.print.a .Values.print.b }}

  ternary:
    case1: {{ .Values.ternary.case1 | ternary "1" "2" }}
    case2: {{ .Values.ternary.case2 | ternary "1" "2" }}

  indent:
    indent: 
{{ .Values.data | toYaml | indent 4 }}
    nindent1: {{ .Values.data | toYaml | nindent 4 }}
    nindent2:
    {{- .Values.data | toYaml | nindent 4 }}
          
  default: # Number:0, String: "", List: [], Object: {}, Boolean: false, Null
    nil:     {{ .Values.default.nil     | default "default" }}
    list:    {{ .Values.default.list    | default (list "default1" "default2") | toYaml | nindent 6}}
    object:  {{ .Values.default.object  | default "default:1" | toYaml | nindent 6 }}
    number:  {{ .Values.default.number  | default 1 }}
    string:  {{ .Values.default.string  | default "default" }}
    boolean: {{ .Values.default.boolean | default true }}

  trim:
    trim:       {{ trim "  hello " }}
    trimPrefix: {{ trimPrefix "-" "-hello" }}
    trimSuffix: {{ trimSuffix "-" "hello-" }}

  random:
    randAlphaNum: {{ randAlphaNum 5 }}   # 0-9a-zA-Z
    randAlpha:    {{ randAlpha 5 }}      # a-zA-Z
    randNumeric:  {{ randNumeric 5 }}    # 0-9
    randAscii:    {{ randAscii 5 }}      # ASCII characters

  trunc:    {{ trunc 5 "hello world" }}
  replace:  {{ "hello world" | replace " " "-" }}
  contains: {{ contains "cat" "catch" }}
  b64enc:   {{ b64enc "hello" }}
```

**data 출력 결과**

```yaml
data:
  print:
    print:  Hard Cording
    printf: 1-2

  ternary:
    case1: 1
    case2: 2

  indent:
    indent:
    - a
    - b
    - c
    nindent1:
    - a
    - b
    - c
    nindent2:
    - a
    - b
    - c

  default: # Number:0, String: "", List: [], Object: {}, Boolean: false, Null
    nil:     default
    list:
      - default1
      - default2
    object:
      default:1
    number:  1
    string:  default
    boolean: true

  trim:
    trim:       hello
    trimPrefix: hello
    trimSuffix: hello

  random:
    randAlphaNum: Bfqxq   # 0-9a-zA-Z
    randAlpha:    MWCKm      # a-zA-Z
    randNumeric:  79598    # 0-9
    randAscii:    bsMqA      # ASCII characters

  trunc:    hello
  replace:  hello-world
  contains: true
  b64enc:   aGVsbG8=
```

## 지역변수

**cm4-1.yaml**

```yml
apiVersion: v1
kind: ConfigMap
metadata:
  name: variables-with
data:
  dev:
  {{- $relname := .Release.Name -}}
  {{- with .Values.dev }}
    env: {{ .env }}
    release: {{ $relname }}
    log: {{ .log }}
  {{- end }}
```

- relname 지역변수에 Release.Name을 저장하는데 변수를 지정하지 않고 다음과 같이 작성하면 에러 발생

```yml
data:
  dev:
  {{- with .Values.dev }}
    release: {{ .Release.Name }}
  {{- end }}
```

- 이유는 with를 쓰게 되면 with 안에서만 찾기 때문에 Release.Name을 찾을 수 없음

**data 출력 결과**

```yml
data:
  dev:
    env: dev
    release: mychart
    log: info
```

## Range

**cm4-2.yaml**

```yml
apiVersion: v1
kind: ConfigMap
metadata:
  name: variables-range
data:
  # for (int i ; i=list.length ; i++) { printf "i : list[i]" };
  index:
  {{- range $index, $value := .Values.data }}
    {{ $index }}: {{ $value }}
  {{- end }}

  # for (Map<key, value> map : list) { printf "map.key() : map.value()" };
  key-value:
  {{- range $key, $value := .Values.dev }}
    {{ $key }}: {{ $value | quote }}
  {{- end }}
```
**data 출력 결과**

```yml
data:
  # for (int i ; i=list.length ; i++) { printf "i : list[i]" };
  index:
    0: a
    1: b
    2: c

  # for (Map<key, value> map : list) { printf "map.key() : map.value()" };
  key-value:
    env: "dev"
    log: "info"
```

<span style="font-size:70%">[참조]: 인프런 - 대세는 쿠버네티스 [Helm편]

끝!
