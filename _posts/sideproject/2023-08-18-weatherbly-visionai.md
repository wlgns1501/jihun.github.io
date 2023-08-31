---
layout: single
title: "[weathervely 6] 구글 vision ai"
categories: sideproject
tag: [Side-Project, weathervely]
toc: true
---

# 왜 사용하려 하는가?

쇼핑몰 사이트에 코디 탭이 없는 사이트도 있고, 코디 탭이 있다 한들 이미지를 사용할 수 없는 상황이 발생한다. 그래서 이 부분을 해소하고자 vision ai를 사용하려 한다.

# 구글 vision ai

이 api는 이미지를 분석을 하여 라벨링을 해주는 api다. 얼마나 정확한지는 모르겠으나, 한 번 사용해보려 한다.

## 문제 정의

코디 이미지를 봤을 때, 우리는 기본적으로 한 아이템을 알고 있다. 예를 들어 코디 이미지가 상의 - 하의 를 입고 있고, 우리는 상의의 정보를 알고 있다면 하의의 정보만 가져오면 된다. 비록 이 하의의 아이템 코드, 카테고리 정보, 기장에 대한 정보를 모르더라도, 어떻게든 매핑만 시키면 된다.

## api 사용

사용법은 간단하다. 구글 developer에 api를 등록 시키고, api 키의 정보를 담고 있는 json 파일을 이용하면 된다.

1. google cloud vision ai를 인스톨 한다.
2. google cloud의 접속 정보를 불러온다.

```python
os.environ["GOOGLE_APPLICATION_CREDENTIALS"] = "weatherbly-8177ae707a88.json"
```

3. image 라벨링 한다.

```python

for image_url in self.image_urls:
            try:
                response = self.client.annotate_image(
                    request={
                        "image": {
                            "source": {"image_uri": image_url},
                        },
                    }
                )

                print(response)

                labels = response.label_annotations

                for label in labels:
                    self.labels.append(label.description)

                print(self.labels)
                self.json_data["image_url"] = image_url
                self.json_data["labels"] = self.labels

                print(self.json_data)
            except Exception as e:
                print(e)
```

이렇게 사용하면 이미지 라벨링을 해주게 된다.

## 결과

3개의 아이템의 이미지들을 라벨링 시키고 엑셀 파일을 만들었다.

<img src="/assets/images/vision_ai.png">

필요 없는 라벨을 지우고, 실제로 사용 가능한 것들을 노란색의 바탕으로 해놨다. 그런데 우리가 사용할 수 있는 이미지인지, 정확성이 얼마나 높은지를 따져봤을 때,
한 아이템당 라벨링 분석이 제대로 된 것들이 3 ~ 4개 꼴이고, 그 중 사용 가능한 이미지들이 1개 정도 밖에 없다. 그래서 여러 아이템을 넣어 봤을 때, 과연 우리가 코디 - 아이템의 매핑을 할 수 있는 것들이 얼마나 있을까 라는 것을 생각해 봤을 때, 거의 없다고 생각이 들었다.

## 대안책

1. google vertex ai

   학습이 가능한 ai이다. 이미지 라벨링도 물론 할 수 있다. 하지만 비용이 상당하다.

2. admin 페이지 개설

   크롤링을 하여 clothes의 데이터는 넣고, admin 페이지에서 closet 테이블의 데이터를 넣을 수 있게끔 한다. 코디 이미지 선택을 하고, 크롤링 하는 시간을 줄일 수 있다.
   이 후 입점하는 쇼핑몰에게 아이디를 부여하여 데이터를 직접 넣도록 한다.

3. 라벨링 툴

   이미지 라벨링 노코드 툴이 있다. 이 또한 학습이 가능하다. 하지만 세팅을 하는데 시간이 오래 걸린다.

이런 대안책을 생각해 봤다. 현실적으로 크롤링을 통해서 데이터를 가공하는 것은 불가능 하다고 판단이 들었고, 언제까지 이렇게 시간을 들일 수 없다. 회의를 통해서 1차 배포 후 admin 페이지를 생성하는 것으로 선택하게 되었다. 비용측면도 안들고, 확장 가능성도 있다고 판단이 들었다.

## 코디 - 아이템 형식 변경

현실적으로 현재로썬 코디의 모든 하위 아이템을 넣을 수 없다. 쇼핑몰에도 우리가 원하는 형식으로 엑셀 파일을 만들어 문의를 하였는데 아무도 답장을 해주지 않았다.
그래서 일단 하나의 아이템만 넣게끔 하기로 했다.

# 느낀점

이미지 라벨링 ai만 있으면 되겠지 라고 생각을 했는데, 생각보다 라벨링이 잘 되지 않아 많이 속상했다. 어떻게든 우리가 원하는 데이터로 넣고 싶었고, 기존 기획대로 하고 싶었는데 많이 아쉽다. 이 부분을 어떻게 하면 가져올 수 있을까 여러가지 시도를 해봐야 할 것 같다. 꼭 기술적으로 해결이 안되더라도 다른 방법으로 해결이 될 수 있게끔 노력을 해야 겠다.
