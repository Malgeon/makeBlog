---
layout: post
author: study
title:  "슬라이드 효과"
description: "CSS에서 슬라이드 효과 구현"
categories: [ Study ]
tags: [C, Java]
---

# 이미지 슬라이드

 FE를 구성하다보면 가장 대표적으로 필요한 기능이 바로 슬라이드다.
 버튼식 슬라이드, 무한 슬라이드 등등. 

 찾아보니 역시 방법은 천차만별이어서 공부한 바를 작성해 둔다.
 
## width : Calc()
 사용하고자 할 자식노드들의(이미지) width를 모두 25%로 맞춘다음
 해당 부모노드에서 width를 4배한 값 calc(400%)으로 맞추면 이미지들은 100%가 되게 된다. 

 이때 margin-left를 -100 * n % 순서대로 바꾸게 되면 각 이미지들을 가리키게 된다.
 
 아주 간편하다.

 `기준점이 설정되어 있기 때문에 한번은 초기화를 해줘야 한다.`

## translate3d() 이용
 width :Calc와 같은 매커니즘이다.
transform의 translate3d(x축, y축, z축) 에서 조절하고 싶은 축을 조절하면 된다.
 

