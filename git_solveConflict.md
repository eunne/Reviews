# 충돌 해결하기

이번에 해결해야 할 과제이다.

오늘 git을 처음봐서 지금 내게는 '충돌여부'가 중요한게 아니라 '기본개념'이 중요하기에 스터디 진행 할 때는 신경도 안 썼지만 현재 어느정도 학습을 하고나니 이제 충돌에 대해 해결해보려 한다.

각각 개인브랜치를 만들어서 taiwan이라는 브랜치에 병합하려하니 conflict가 발생한다. 왜냐면 각각 브랜치에서 변경한 내용이 [README.md](http://readme.md/) 파일의 같은 행에 존재하고 있기 때문이다.

그림

실제 conflict가 발생한 내용을 보니 다음과 같았다.

![/img/conflict.JPG](/img/conflict.JPG)

```
<<<<<<< fenglisu
# 펑리수

1. 풀어진 버터에 설탕, 달걀노른자를 넣고 섞는다.
2. 가루류를 체쳐 넣고 잘 섞은 뒤 뭉쳐 냉장고에 30분간 휴지시킨다.
3. 통조림 파인애플은 작게 다진다.
=======
# 우육면

1. 청경채는 통째 반으로 잘라 준비해주세요.
2. 냄비에 오뚜기 옛날 사골곰탕과 분량의 물을 붓고 센불에 끓여주세요.
3. 국물이 끓어오르면 이금기 굴소스 1/2큰술과 오뚜기 순후추를 기호에 맞게 넣어주세요.
>>>>>>> taiwan

```

<br/>

fenglisu라는 브랜치와 taiwan이라는 브랜치 간의 병합에서 해당 오류가 발생했다는 메세지가 떴다.

<br/>

merge를 해버리면 fast-forward병합이 되서 taiwan 브랜치가 우육면branch쪽으로 옮겨와 병합이 되는데 우육면과 펑리수 브랜치가 같은 버전? 단계?에 놓여있기 때문에 병합을 하면 위와같이 충돌이 발생하게 된다.

지금 git에 대해 공부하고 있는 사이트에 따르면, 충돌이 일어난 파일을 일일이 확인해서 수정해줘야 한다는데 그렇다면 이렇게 수정하면 되는건가?

<br/>

```

# 펑리수

1. 풀어진 버터에 설탕, 달걀노른자를 넣고 섞는다.
2. 가루류를 체쳐 넣고 잘 섞은 뒤 뭉쳐 냉장고에 30분간 휴지시킨다.
3. 통조림 파인애플은 작게 다진다.

# 우육면

1. 청경채는 통째 반으로 잘라 준비해주세요.
2. 냄비에 오뚜기 옛날 사골곰탕과 분량의 물을 붓고 센불에 끓여주세요.
3. 국물이 끓어오르면 이금기 굴소스 1/2큰술과 오뚜기 순후추를 기호에 맞게 넣어주세요.

```

<br/>

ㅇㅅㅇ?

진짜?

일단, 이렇게 직접 충돌부분을 수정하게 되면 해당 변화를 기록하는 병합커밋이 새로 생성이 되고 taiwan브랜치가 한단계 더 나아간 모습을 보여주게 된다. (실제 그런지는 모르겠는데 이론상으론 그렇다.)

<br/>
<br/>

### rebase를 사용해보자

수행과제에 rebase를 이용해보라는 힌트가 있었다.

rebase를  이용하면 이력을 하나의 줄기로 만들 수 있다고 한다. 근데 수정은 직접 손으로 하는 것 같다.

<br/>

- merge와 rebase 비교

    ![/img/conflict2.PNG](/img/conflict2.PNG)

<br/>
<br/>

뭐랄까.

충돌을 해결하기 전에 전체 흐름이 이해가 되는 듯 하면서도 아닌 듯해서 정리를 하고 가야할 듯 하다.

<br/>

# PR 먼저 보내보자.

1. next-step 원본저장소를 나의 원격저장소로 fork하여 가져온다.

<br/>

2. 나의 원격저장소에서 컴퓨터 상의 로컬 저장소로 clone한다. (내 원격저장소가 origin이 된다.)

```markdown
git clone https://github.com/{본인의 GIt name}/git-recipe.git
```

<br/>

3. next-step을 upstream이라는 이름으로 원본저장소를 등록한다.

```markdown
git remote add upstream {next-step 원본저장소 url주소}
```

<br/>

4. 나의 원격저장소에서 taiwan이란 브랜치를 기준으로 fenglisu라는 나의 브랜치를 만든다. (..그러면  taiwan 브랜치 내에 fenglisu라는 브랜치가 생기나?)

```markdown
git switch taiwan
git switch -c fenglisu (-c는 create를 의미한다)
* git checkout -b fenglisu랑 같은 의미인 듯, 하지만 checkout은 레거시라했다.
```

<br/>

5. fenglisu 브랜치에서 레시피를 하나씩 등록(add & commit)하는 작업을 진행한다.

```markdown
git add README.md
git commit -m "커밋메시지"
```

이번 과제에서는 레시피 3줄을 하나씩 커밋을 날리도록 했기 때문에 한줄치고 (add) 커밋날리고 (commit)를 총 세번 반복했다.

<br/>

6. 커밋이 모두 완료되면 나의 원격저장소에 push를 해준다

```markdown
git push -u origin fenglisu
```

github 상에 존재하는 나의 원격저장소에는 fenglisu라는 브랜치가 없기 때문에 위와 같이 옵션을 설정해 줘야 한다. 그런데 난 나의 원격저장소에서 fenglisu브랜치를 만들어버려서 그냥 바로 push를 날렸던 것 같다... (기억이 안나)

<br/>

7. 그리고 원본저장소에 pull request를 날린다.

이 작업은 github에서 직접 행한다.

<br/>
<br/>

# 충돌발생!!! 해결해보자!!!!

1. 원본저장소(upstream)에서 taiwan이라는 브랜치의 최신이력을 가져온다.

```markdown
git fetch upstream taiwan
```

fetch를 이용하는 이유는 원본저장소의 내용만 확인하고 로컬데이터와는 병합하고 싶지 않을 때 이용한다.

fetch를 실행하면 원본저장소의 최신이력을 확인할 수 있다.

현재 upstream의 taiwan 브랜치에는 conflict 된 내용이 있으니 해당 내용을 가져오는 것이다.

<br/>

2. 원본저장소의 taiwan브랜치의 내용을 rebase를 이용해서 수정하자.

```markdown
git rebase upstream/taiwan
```

근데 궁금한게, 그러면 내 local저장소는?

흠, 보니까 이전에 로컬저장소에서 taiwan에 fenglisu를 만들어서 내 로컬저장소의 taiwan에 원본저장소를 merge한 후 내 로컬에서 작업을 해줘야하는건가?

<br/>
<br/>

## [첫번째 시도] rebase 뭐가 문제야, 취소해보자.

망고빙수를 먼저 merge한 후에 나의 펑리수를 rebase를 했는데 첫번째 줄을 커밋을 하고 rebase —continue를 했더니 rebase상태를 벗어나 버렸다. 그리고 세번째 레시피까지 모두 추가되었다. 왜지??

창섭님의 말로는 이전에 rebase했던 이력이 남아있어서 그런게 아닐까라고 하셨는데 한번 확인해보긴 해야할 것 같은데 어떻게..?

일단은 rebase를 취소해보기로 한다.

<br/>

```markdown
git reflog 브랜치이름
```

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8aa8435e-e000-425b-95a8-8797af85af37/rebase_reset.png](/img/rebase_reset.png)

위와같이 로그가 떴다. 아예 처음으로 돌아가기 위해서 fenglisu{0}으로 이동하기로 한다.

<br/>

```markdown
git reset --hard fenglisu@{0}
```

..ㅠㅠ 왜 rebase 전으로 안 돌아가는 걸까. 

upstream에서 받아온 그 상태 그대로로 돌아가고 싶은데 안된다. 뭔가 이미 README.md의 conflict가 해결 된 상황 전으로 돌아가지지가 않는다. 왜죠?ㅠㅠ

<br/>
<br/>

## [두번째 시도] 왜 펑리수끼리 충돌하는건데?

결국 이유를 찾지 못하고 다시 clone으로 받아온다.

이번에는 vscode를 실행시켜서 해봤는데 1번까지는 무난하고 2번에서 충돌이 발생을 해서 보니 펑리수끼리 충돌이 발생해있었다. 음? 나는 망고빙수랑 충돌이 일어날 줄 알았는데 1번펑리수랑 1번,2번 펑리수가 충돌???

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2a1dd43e-7643-4865-8fd3-082db15b1758/conflict2.png](/img/conflict2.png)

이건 대체 뭔 상황이여....................

이 문제를 놓고 열심히 토론을 나눴지만 결국 뭐가 문제인지 찾진 못했다.. 혹시 1번 적고 merge하고 2번 적고 merge하는 형식이 아닐까?라는 의견이 나와서 push를 하려 했지만 나는 이미 두번째 rebase상태에 진입해 있어서 push가 안됐다.

그래서 rebase상태를 —quit으로 나가고 나서 push를 시도했더니 이번에는 merge가 필요하다고 한다.  (need merge)

이럴 땐 구글선생님!

선생님이 '응 그러면 reset merge를 하면돼^^'라고 해서 했더니 뭐시여?! 망고빙수가 없어졌다???

그래서 다시 fetch로 가져왔더니 이번엔 펑리수가 망고빙수 위에 존재해버리는 형태가 되어버렸다. (원래는 망고빙수 아래에 펑리수가 있어야함)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3c387a9d-9849-4274-ae53-08e80d62578e/reverse.png](/img/reverse.png)

혼돈의 카오스..ㄷㄷ

이건 reset으로도 복구가 어려울 것 같아...ㄷㄷ

결국 싹 없애버리고 다시 clone을 하기로 한다.

<br/>
<br/>

## [세번째 시도] 결국 첫번째가 옳았도다.

step by step으로 차근히 시도를 했다.

일단, 내 예상대로 망고빙수와 conflict가 일어났다. 아마 망고빙수와 펑리수가 같은 레벨에 존재하기 때문인 것 같다. 오케이 좋았어..

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fb7e2c51-a391-42a1-bc26-cdf3d1a31bdf/conflictv2-1.png](/img/conflictv2-1.png)

하지만, 첫번째 시도때와 마찬가지로 첫번째 rebase 상태에서 충돌을 해결하자 두세번째가 그냥 해결된 README.md가 짠! 하고 나타났다.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ca275598-121c-4a82-8b3e-50d634203caf/README.png](/img/README.png)

대체 왜 그런겁니까.. README씨..

그런데, 그 순간 창섭님이 아! 라는 깨달음의 감탄사를 내뱉었다. 오오 뭐죠?!!

<br/>
<br/>

## 단 하나의 커밋만 일어나는 이유를 발견한 것 같다. (뇌피셜)

rebase는 커밋을 하나로 정리하는 기능이다.

그래서 창섭님의 망고빙수가 rebase를 통해 merge가 됐기 때문에 처음에 우리가 3개의 커밋을 날렸던 것이 하나로 정리된 것이다.

즉, 최초 rebase를 하면서 커밋이 하나로 정리가 된 것이다.

거기에 내가 conflict를 해결하니 비록 1번의 충돌만 해결했지만 비록 재가 2,3번을 직접적으로 수정하진 않았지만 2,3번이 자동적으로 해결이 된 것이다. 물론, 이건 뇌피셜이어서 정말 그런진 모르겠지만 일단 충돌이 해결된건 확실하다고 한다.

근데 내가 bash에서 rebase를 하면 왜 (1/3) 이렇게 뜨는가,에대해 말하자면 rebase를 하지만 커밋을 삭제하지 않고 단지 사용하지 않도록 내버려두기에 그 정보를 보여주는 것이 아닌가 싶다라고 했다. 물론, 참고만 하고 나중에 정확히 알게되면 교정하는 것으로 하기로 했다.

와... 이제 이해가 된다.

처음에 망고빙수 merge 전에 conflict해결하기 위해 commit을 세번 했었는데 rebase된 망고빙수 후에 conflict를 해결하려하니 commit을 한번한 해도 되는지. 바로 rebase가 커밋을 하나로 만들어 버렸기(버리기) 때문이다. 아하.. 대박

세부적인건 모르겠지만 전체적인 부분은 맞는 것 같다. 

그렇게 다사다난한 작업이 끝났고 현재는 merge요청해놓은 상황.

잘 merge됐으면 좋겠드아.

<br/>
<br/>

## 참고

[guide](https://www.notion.so/Git-7c0606b8b87e4861ab57576a22906833)

[solution](https://edu.nextstep.camp/s/pycbkTaz/ls/Hjhasvn4)
