
# Make the Word Cloud #1
## amueller의 wordcloud package를 이용한 mask를 활용한 wordcloud 만들기! 

#### 참고:
#### [1] https://github.com/amueller/word_cloud
#### [2] http://minimaxir.com/2016/05/wordclouds/
#### [3] https://github.com/minimaxir/stylistic-word-clouds/blob/master/wordcloud_github.py

## 1. Package import & font, mask, message setup!
### 이 코드는 mask를 다루는 부분, font를 다루는 부분, word를 다루는 부분으로 나뉜다. 먼저 필요한 패키지들을 불러오는데, numpy와 random, palettable은 font 색 설정할 때, PIL은 mask 이미지를 처리할 때, wordcloud는 이 모든 것을 이용해서 word cloud를 그릴 때 이용된다.
### 아래에서 딱히 어려운 점은 없고, color_func가 여기서는 Dark2_8이라는 palette를 이용해서 색을 만들어낸다. Palette를 바꾸고 싶다면 이 부분을 바꾸면 될 것!
### 여기서 사용한 example은 윤동주 시인의 작품 전체와, 궁서체의 '동주'라는 (1024, 512) pixel의 이미지를 사용하였다.
#### *PIL은 python3에서는 Pillow로 설치하고 PIL로 사용하면 된다.
#### *mask로 쓸 이미지는 png파일로 우리가 단어를 배치할 공간만 그림이 존재해야 한다. 나머지 공간엔 흰색이 아니라 아예 이미지가 없는 png파일이 필요하다.


```python
import numpy as np
import random
from PIL import Image
from wordcloud import WordCloud, STOPWORDS
from palettable.colorbrewer.qualitative import Dark2_8

def color_func(word, font_size, position, orientation, random_state=None, **kwargs):
    return tuple(Dark2_8.colors[random.randint(0,7)])
font = "Hunf"
font_path = "%s.ttf" % font

icon = "Mask_YDJ"
icon_path = "%s.png" % icon

f = open("poem_YDJ.txt", 'r')
message = f.read()
print(message)
f.close()
```

    ﻿ 쉽게 씌여진 시
     
    창밖에 밤비가 속살거려 
    륙첩방은 남의 나라,
    
    시인이란 슬픈 천명인줄 알면서도
    한줄 시를 적어 볼가,
    
    땀내와 사랑내 포근히 품긴
    보내주신 학비봉투를 받아
    
    대학 노-트를 끼고
    늙은 교수의 강으 들으러 간다.
    
    생각해보면 어린 때 동무를
    하나, 둘, 죄다 잃어버리고
    
    나는 무얼 바라
    나는 다만, 홀로 침전하는것일까?
    
    인생은 살기 어렵다는데
    시가 이렇게 쉽게 씌여지는것은
    부끄러운 일이다.
    
    륙첩방은 남으 나라
    창밖에 밤비가 속살거리는데,
    
    등불을 밝혀 어둠을 조금 내몰고,
    시대처럼 올 아침을 기다리는 최후의 나
    
    나는 나에게 작은 손을 내밀어
    눈물과 위안으로 잡는 최초의 악수. 
    
     
     
    
     참회록 
    파란 녹이 낀 구리거울속에
    내 얼굴이 남아있는것은
    어느 왕조의 유물이기게
    이다지도 욕될가
    
    나는 나의 참회의 글을 한줄에 줄이자
    - 만 24년 1개월을
    - 무슨 기쁨을 바라 살아왔던가
    
    래일이나 모레나 그 어느 즐거운 날에
    나는 또 한줄의 참회록을 써야 한다
    - 그때 그 젊은 나이에
    - 왜 그런 부끄러운 고백을 했던가
    
    밤이면 밤마다 나의 거울을
    손바닥으로 발바닥으로 닦아보자
    그러면 어느 운석밑으로 홀로 걸어가는
    슬픈 사람의 뒤모양이
    거울속에 나타나온다 
    
    
     
    
     또 다른 고향 
     
    고향에 돌아온 날 밤에 
    내 백골이 따라와 한방에 누웠다 
    
    어둔 방은 우주로 통하고 
    하늘에선가 소리처럼 바람이 불어온다 
    
    어둠속에 곱게 풍화작용하는 
    백골을 들여다보며 
    눈물 짓는것이 내가 우는것이냐 
    백골이 우는것이냐 
    아름다운 혼이 우는것이냐 
    
    지조 높은 개는 
    밤을 새워 어둠을 짖는다. 
    어둠을 짖는 개는 
    나를 쫓는것일게다 
    
    가자 가자 
    쫓기우는 사람처럼 가자 
    백골 몰래 
    아름다운 또 다른 고향에 가자 
    
    
     
    
     빗자루 
    
    요오리 조리 베면 저고리 되고 
    이이렇게 베면 큰 총 되지. 
    
    누나하고 나하고 
    가위로 종이 쏠았더니 
    어머니가 빗자루 들고 
    누나 하나 나 하나 
    엉덩이를 때렸소 
    
    방바닥이 어지럽다고 
    아아니 아니 
    고놈의 빗자루가 
    방바닥 쓸기 싫으니 
    그랬지 그랬어 
    
    괘씸하여 벽장 속에 감췄드니 
    이튿날 아침 빗자루가 없다고 
    어머니가 야단이지요. 
    
    
     
    
     무서운 시간 
    
    거 나를 부르는 것이 누구요,
    
    가랑잎 이파리 푸르러 나오는 그늘인데,
    나 아직 여기 호흡이 남아 있소.
    
    한 번도 손들어 보지 못한 나를
    손들어 표할 하늘도 없는 나를
    
    어디에 내 한 몸 둘 하늘이 있어
    나를 부르는 것이오.
    
    일을 마치고 내 죽는 날 아침에는
    서럽지도 않은 가랑잎이 떨어질 텐데 ……
    
    나를 부르지 마오.
    
    
     
    
     코스모스 
    
    청초한 코스모스는
    오직 하나인 나의 아가씨,
    
    달빛이 싸늘히 추운 밤이면
    옛 소녀가 못 견디게 그리워
    코스모스 핀 정원으로 찾아간다.
    
    모스모스는
    귀또리 울음에도 수집어지고,
    코스모스 앞에 선 나는
    어렸을 적처럼 부끄러워지나니,
    
    내 마음은 모스모스의 마음이오
    코스모스의 마음은 내 마음이다.
    
    
    
     
    
     소낙비 
    
    번개, 뇌성, 왁자지끈 뚜다려
    머 - ㄴ 도회지에 낙뢰(落雷)가 있어만 싶다.
    
    벼루짱 엎어논 하늘로
    살 같은 비가 살처럼 쏟아진다.
    
    손바닥만한 나의 정원이
    마음같이 흐린 호수되기 일쑤이다.
    
    바람이 팽이처럼 돈다.
    나무가 머리를 이루잡지 못한다.
    
    내 경건한 마음을 모셔드려
    노아 때 하늘을 한모금 마시다
    
    
    
     
    
     비 오는 밤 
    
    솨! 철썩! 파도소리 문살에 부서져 
    잠 살포시 꿈이 흩어진다. 
    
    잠은 한낱 검은 고래 떼처럼 설레어, 
    달랠 아무런 재주도 없다. 
    
    불을 밝혀 잠옷을 정성스레 여미는 
    삼경 
    념원(念願). 
    
    동경의 땅 강남에 또 홍수질 것만 싶어 
    바다의 향수보다 더 호젓해진다. 
    
    
    
     
    
     사랑스런 추억 
    
    봄이 오던 아침, 서울 어느 쪼그만 정거장에서
    희망과 사랑처럼 가차를 기다려,
    
    나를 플랫포옴에 간신히 그림자를 떨어뜨리고,
    담배를 피웠다.
    
    내 그림자는 담배 연기 그림자를 날리고
    비둘기 한 체가 부끄러울 것도 없이
    나래 속을 속, 속, 해빛에 비춰 날았다.
    
    기차는 아무 새로운 소식도 없이
    나를 멀리 실어다 주어,
    
    봄은 다 가고 - 동경(東京)교외 어느 조용한 하숙방에서,
    옛 거리에 남은 나를 희망과 사랑처럼 그리워한다.
    
    오늘도 기차는 몇 번이나 무의미하게 지나가고
    오늘도 나는 누구를 기다려 정거장 가차운 언덕에서
    서성거릴게다.
    
    - 아아 젊음은 오래 거기 남아 있거라.
    
    
     
    
     사랑의 전당(殿堂) 
    
    순아 너는 내 전(殿)에 들어 왔든 것이냐?
    내사 언제 네 전에 들어왔든 것이냐?
    
    우리들의 전당은
    고풍(古風)한 풍습이 어린 사랑의 전당
    
    순아 암사슴처럼 수정눈을 나려감어라,
    난 사자처럼 엉크린 머리를 고루련다.
    우리들의 사랑은 한낱 벙어리었다.
    
    성스런 촛대에 열(熱)한 불이 꺼지기 전
    순아 너는 앞문으로 내 달려라.
    
    어둠과 바람이 우리 창에 부닥치기 전
    나는 영원한 사랑을 안은 채
    뒷문으로 멀리 사라지련다.
    
    이제 네게는 삼림(森林)속의 아늑한 호수가 있고
    내게는 준(峻)한 산맥이 있다.
    
    
     
    
     황혼(黃昏)이 바다가 되여 
    
    하로도 검푸른 물결에
    흐느적 잠기고 …… 잠기고 ……
    
    저 웬 검은 고기떼가
    물든 바다를 날아 횡단(橫斷)할고.
    
    낙엽이 된 해초(海草)
    해초마다 슬프기도 하오.
    
    서공에 걸린 해말간 풍경화
    옷고름 너어는 고아의 서름.
    
    이제 첫 항해하는 마음을 먹고
    방바닥에 나딩구오 … 딩구오 …
    
    황혼이 바다가 되어
    오늘도 수많은 배가
    나와 함께 이 물결에 잠겼을 게오.
    
    
     
    
     산림(山林) 
    
    시계가 자근자근 가슴을 때려
    불안한 마음을 산림이 부른다.
    
    천년 오래인 연륜에 찌들은 유암한 산림이,
    고달픈 한몸을 포옹할 인연을 가졌나보다.
    
    산림의 검은 파동우으로부터
    어둠은 어린 가슴을 짓밟고
    이파리를 흔드는 저녁바람이
    솨 공포에 떨게 한다.
    
    멀리 첫여름의 개고리 재질댐에
    흘러간 마을의 과거은 아질타.
    
    나무틈으로 반짝이는 별만이
    새날의 희망으로 나를 이끈다.
    
    
     
     
    
     바다 
    
    실어다 뿌리는 
    바람처럼 씨원타. 
    
    솔나무 가지마다 새침히 
    고대를 돌리어 뻐들어지고, 
    
    밀치고 
    밀치운다. 
    
    이랑을 넘는 물결은 
    폭포처럼 피어오른다. 
    
    해변에 아이들이 모인다. 
    찰찰 손을 씻고 구보로. 
    
    바다는 자꾸 섧어진다. 
    갈매기의 노래에 …… 
    돌아도보고 돌아다보고 
    돌아가는 오늘의 바다여 
    
    
     
    
     풍경 
    
    봄바람을 등진 초록빛 바다
    쏟아질듯 쏟아질듯 위태롭다.
    
    잔주름 치마폭의 두둥실거리는 물결은,
    오스라질듯 한끝 경쾌롭다.
    
    마스트 끝에 붉은 기ㅅ발이
    여인의 머리칼처럼 나부낀다.
    
    이 생생한 풍경을 앞세우며 뒤세우며
    외-ㄴ하루 거닐고 싶다.
    
    - 우중충한 오월(五月)하늘 아래로,
    - 바닷빛 포기포기에 소놓은 언덕으로.
    
    
     
    
     별헤는 밤 
    
    계절이 자나가는 하늘에는 가을로 가득 차 있읍니다.
    
    나는 아무 걱정도 없이
    가을 속의 별들을 다 헤일듯 합니다
    
    가슴속에 하나 둘 새겨지는 별을
    이제 다 못헤는 것은
    쉬이 아침이 오는 까닭이오.
    아직 나의 청춘이 다하지 않는 까닭입니다.
    
    별 하나에 추억과
    별 하나에 사랑과
    별 하나에 쓸쓸함과
    별 하나에 동경과
    별 하나에 시와
    별 하나에 어머니, 어머니.
    
    어머님,
    그리고 당신은 멀리 북간도에 계십니다.
    
    나는 무엇인지 그리워
    이 많은 별빛이 나린 언덕우에
    내 이름자를 써 보고
    흙으로 덮어 버리었읍니다.
    
    단은 밤을 새워 우는 벌레는
    부끄러운 이름을 슬퍼하는 까닭입니다.
    
    그러나 겨울이 지나고 나의 별에도 봄이 오면
    무덤우에 파란 잔디가 피어나듯이
    내 이름자 묻힌 언덕 위에도
    자랑처럼 풀이 무성할 게외다.
    
    
    
    
    
     삶과 죽음
    
    삶은 오들도 죽음의 서곡을 노래하였다.
    이 노래가 언제나 끝나랴
    
    세상 사람은 -
    뼈를 녹여내는 듯한 삶의 노래에
    춤을 춘다.
    사람들은 해가 넘어가기 전
    이 노래 끝의 공포를
    생각할 사이가 없었다.
    
    하늘 복판에 알 새기듯이
    이 노래를 부른 자가 누구뇨
    
    그리고 소낙비 그친 뒤 같이도
    이 노래를 그친 자가 누구뇨
    
    죽고 뼈만 남은
    죽음의 승리자 위인들!
    
    
     
    
     이런날 
    
    사이좋은 정문의 두 돌기둥 끝에서
    오색기와 태양기가 춤을 추는 날,
    금을 그은 지역의 아이들이 즐거워하다.
    
    아이들에게 하로의 건조한 학과(學課)로
    해말간 권태(倦怠)가 깃들고
    '모순(矛盾)'두 자를 이해치 못하도록
    머리가 단순하였구나.
    
    이런 날에는
    잃어버린 완고하던 형을
    부르고 싶다.
    
    
     
    
     돌아와 보는 밤 
    
    세상으로부터 돌아오듯이 이제 내 좁은 방에 돌아와
    불을 끄옵니다. 불을 켜두는 것은 너무나 피로롭은
    일이옵니다. 그것은 낮의 연장이옵기에 -
    
    이제 창을 열어 공기를 바꾸어 들여야 할텐데 밖을 가만히
    내다보아야 방안과 같이 어두워 꼭 세상 같은데 비를 맞고 오든
    길이 그대로 비속에 젖어 있사옵니다.
    
    하루의 울분을 씻을 바 없어 가만히 눈을 감으면 마음속으로
    흐르는 소리, 이제, 사상이 능금처럼 저절로 익어 가옵니다.
    
    
     
    
     어머니 
    
    어머니
    젖을 빨려 이 마음을 달래여주시오
    이 밤이 자꾸 설혀 지나이다.
    
    이 아니는 턱에 수염자리 잡히도록
    무엇을 먹고 살았나이까?
    오늘도 한주먹이
    입에 그대로 물려있나이다
    
    어머니
    부서진 랍인형도 쓰러진지
    벌써 오랩니다.
    철비가 우우주군히 내리는 이 밤을
    주먹이나 빨면서 새우릿가?
    어머니! 그 어진 손으로
    이 울음을 달래여주시오
    
    
    
     
    
     서시 
    
    죽는 날까지 하늘을 우러러
    한 점 부끄럼이 없기를.
    잎새에 이는 바람에도
    나는 괴로워했다.
    
    별을 노래하는 마음으로
    모든 죽어가는 것을 사랑해야지
    그리고 나한테 주어진 길을
    걸어가야겠다.
    
    오늘 밤에도 별이 바람에 스치운다.
    
    
     
    
     자화상
    
    산모롱이를 돌아 논가 외딴 우물을 홀로 찾아가선 가만히 들여다봅니다. 
    우물속에는 닭 밝고 구름이 흐르고 하늘이 펼치고 파아란 
    바람이 불고 가을이 있습니다. 
    
    그리고 한 사나이가 있습니다. 
    어쩐지 그 사나이가 미워져 돌아갑니다. 
    
    돌아가다 생각하니 그 사나이가 가엾어집니다. 
    도로 가 들여다보니 사나이는 그대로 있습니다. 
    
    다시 그 사나이가 미워져 돌아갑니다. 
    돌아가다 생각하니 그 사나이가 그리워집니다. 
    
    우물 속에는 달이 밝고 구름이 흐르고 하늘이 펼치고 파아란 
    바람이 불고 가을이 있고 추억처럼 사나이가 있습니다. 
    
    
     
    
     추 억 
    
    봄이 오던 아침, 서울 어느 조그만 정거장에서 
    희망과 사랑처럼 기차를 기다려, 
    
    나는 플랫폼에서 간신한 그림자를 떨어트리고, 
    담배를 피웠다. 
    
    내 그림자는 담배 연기 그림자를 날리고 
    비둘기 한 떼가 부끄러운 것도 없이 
    나래 속을 속, 속, 햇빛에 비춰, 날았다. 
    
    기차는 아무 새로운 소식도 없이 
    나를 멀리 실어다주어, 
    
    봄은 다 가고 ─ 동경 교외 어느 조용한 하숙방에서, 
    옛 거리에 남은 나를 희망과 사랑처럼 그리워한다. 
    
    오늘도 기차는 몇 번이나 무의미하게 지나가고, 
    오늘도 나는 누구를 기다려 정거장 
    가차운 언덕에서 서성거릴 게다. 
    
    ─아아 젊음은 오래 거기 남아 있거라. 
    
    
     
    
     소년 
    
    여기저기서 단풍잎 같은 
    슬픈 가을이 뚝뚝 떨어진다.
    
    단풍잎 떨어져 나온 자리마다 
    봄을 마련해 놓고 나뭇가지
    위에 하늘이 펼쳐있다. 
    
    가만히 하늘을 들여다보려면
    눈썹에 파란 물가이 묻어난다. 
    다시 손바닥을 들여다본다.
    
    소금에는 맑은 강물이 흐르고,
    강물 속에는 사랑처럼 슬픈 얼굴 - 
    아름다운 순이(順伊)의 얼굴이어린다.
    
    소년은 황홀히 눈을 감아본다.
    그래도 맑은 강물은 흘러
    사랑처럼 슬픈 얼굴 -
    아름다운 순이의 얼굴은 어린다.
    
    
    
     
    
     굴 뚝 
    
    산골짜기 오막살이 낮은 굴뚝엔
    몽기몽기 웨인연기 대낮에 솟나,
    
    감자를 굽는 게지 총각애들이
    깜박깜박 검은눈이 모여 앉아서
    입술에 꺼멓게 숯을 바르고
    옛이야기 한커리에 감자 하나씩.
    
    산골짜기 오막살이 낮은 굴뚝엔
    살랑살랑 솟아나네 감자 굽는내.
    
    
    
     
    
     비둘기
    
    안아보고 싶은 귀여운
    산비둘기 일곱 마리
    하늘 끝까지 보일 듯이 맑은 공일날 아침에
    
    벼를 거두어 빤빤한 논에
    앞을 다투어 모이를 주우며
    어려운 이야기를 주고받으오
    
    날씬한 두 나래로 조용한 공기를 흔들어
    두 마리가 나오
    집에 새끼 생각이 나는 모양이오. 
    
    
     
    
     창공
    
    그 여름날
    열정의 포푸라는
    오려는 창공의 푸른 젖가슴을
    어루만지려
    팔을 펼쳐 흔들거렸다.
    끓는 태양 그늘 좁다란 지점에서.
    
    천막같은 하늘 밑에서
    떠들던 소나기
    그리고 번개를,
    
    춤추든 구름은 이끌고
    남방으로 도망하고,
    높다랗게 창공은 한폭으로
    가지우에 퍼지고
    둥근달과 기러기를 불러 왔다.
    
    푸드른 어린 마음이 이상에 타고,
    그의 동경의 날 가을에
    조락의 눈물을 비웃다.
    
    
    
     
    
     산상(山上)
    
    거리가 바둑판처럼 보이고,
    강물이 배암의 시끼처럼 기는
    산 위에가지 왔다.
    아직쯤은 사람들이
    바둑돌처럼 버려 있으리라.
    
    한나절의 태양이
    함석지붕에만 비치고,
    굼벙이 걸음을 하는 기차가
    정거장에 섰다가 검은 내를 토하고
    또 걸음발은 탄다.
    
    덴트 같은 하늘이 무너져
    이 거리 덮을까 궁금하면서
    좀더 높은 데로 올라가고 싶다.
    
    
    
     
    
     아침 
    
    휙, 휙, 휙,
    소꼬리가 부드러운 채찍질로
    어둠을 좇아,
    캄, 캄, 어둠이 깊다깊다 밝으오.
    
    이제 이 동리의 아침이
    풀살 오는 소엉덩이처럼 푸드오.
    이 동리 콩죽 먹은 사람들이
    땀물을 뿌려 이 여름을 길렀오.
    
    잎, 잎, 풀잎마다 땀방울이 맺혔오.
    
    구김살 없는 이 아침을
    심호흡하오 또 하오.
    
    
    
     
    
     봄 
    
    봄이 혈관 속에 시내처럼 흘러
    돌, 돌, 시내 가차운 언덕에
    개나리, 진달래, 노-란 배추꽃,
    
    삼동(三冬)을 참아온 나는
    풀포기처럼 피어난다.
    
    즐거운 종달새야
    어느 이랑에서나 즐거웁게 솟쳐라.
    
    푸르른 하늘은
    아른아른 높기도 한데 …
    
    
    
     
    
     이 올때까지 
    
    다들 죽어가는 사람들에게
    검은 옷을 입히시오.
    
    다들 살아가는 사람들에게
    하얀 옷을 입히시오.
    
    그리고 한 침대에
    가즈런히 재우시요.
    
    다들 울러들랑
    젖을 먹이시요.
    
    이제 새벽이 오면
    나팔소리 들려 올 게외다. 
    
    
    


## 2. Load the mask, font coloring, generate word cloud!
### 먼저 icon에 image를 불러오고, mask를 RGB type으로 icon과 같은 사이즈로 전부 (255,255,255)값 (아마 흰색?)으로 만들어내고, 이 (255,255,255)가 이미지가 채워지지 않는 부분을 의미한다.  (?Image.new를 참고!), 그리고 나서 이 mask에 아까 불러온 icon의 형상을 붙여넣는다. 그리고 나서 이를 nparray로 만든다.
### 이제 만들어놓은 mask와 불러놨던 font를 이용하여 WordCloud함수를 이용해 wordcloud를 만들어내고, WordCloud.recolor를 이용해서 아까 만들어놓은 color_func()를 사용해 불러온 palette를 이용해 색을 입힌다. 그리고 저장하면 끝!


```python
icon = Image.open(icon_path)
mask = Image.new("RGB", icon.size, (255,255,255))
mask.paste(icon,icon)
mask = np.array(mask)

wc = WordCloud(font_path=font_path, background_color="white", max_words=2000, mask=mask,
               max_font_size=300, random_state=42)
               
# generate word cloud
wc.generate_from_text(message)
wc.recolor(color_func=color_func, random_state=3)
wc.to_file("Yun_Dongju.png")
```




    <wordcloud.wordcloud.WordCloud at 0x7f84df397780>



### Additional Tip. [2]에 가면, 올린이의 다양한 작품들을 볼 수 있다. 그리고 해당하는 코드들은 [3]에서 볼 수 있다. [1]에서는 wordcloud의 기본적 사용법들에 대해 알려준다. 참고하기를


```python

```
