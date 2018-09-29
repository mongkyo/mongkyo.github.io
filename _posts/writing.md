터미널에서 touch '파일명' 
바로 빈 파일이 생성된다


import functions 
는 python을 실행한 부분을 루트를 기준으로 인식한다
`__init__`에 있는 파일들을 참조한다 


from . import functions
루트에서 가져온것이 아니라면 위치와 함께 적어줘야 하기 때문에 
from import 를 사용한다

from functions import game == from . import functions

경로를 찾아