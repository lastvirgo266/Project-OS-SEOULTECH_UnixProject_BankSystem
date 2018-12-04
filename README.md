# SEOULTECH_UnixProject_BankSystem


## Proto1차(정승진)
1. 처음에 vfork()를 이용 --> 자식 프로세스가 완료될때까지 부모프로세스는 아무것도 못함  = 병렬연산 안됨
2. vfork +exec 이용 --> exec는 인수를 char 배열형으로 밖에 못넘겨줌(인수들의 주소값 못넘겨줌) = 메모리 공유안됨
3. vfork + exec + 파일입출력 이용 --> exec에 파일의 이름을 넘겨줌으로써 파일을 읽고 쓰고를 통해 변수를 간접적으로 공유 = 원인미상 오류 발생
4. 너무 빨리 프로그램을 읽어서 읽고 쓰는데 시간이 부족해 충돌이 발생한다고 판단해 sleep을 넣어서 프로그램에 충분한 시간 부여 --> 이전의 버그는 해결됬으나 새로운 원인미상 버그 발생
5. fork + exec + 파일입출력 이용 --> 원인미상의 오류 제거, 그러나 while문을 반복할수록 읽고 쓰는게 2배씩 늘어남(프로그램 스택이 늘어남)
6. 아무것도 수행하지 못하는 fork문이 새로운 fork문을 발생한다고 판단 --> 아무것도 수행하지 못하는 fork문은 종료(_exit(0) = 버그 해결)


## Proto2차(정승진)
1. 상태도를 만들어 이중으로 if문을 돌던 버그를 수정함(이제 main에 있는 메모리를 fork를 이용해도 공유가 가능함)(쓰레드 구현성공)


## Proto3차(정승진)
-execl 함수 제거 : 파일을 옮길때마다 PATH 경로값을 구해야함 --> 외부데이터를 참조한 내부 스위치 파일을 구현한 상황에서 굳이 execl 함수를 쓸 이유가 없음


## Proto4차(정승진)
-shmget, shmat 을 사용하여 메모리 공유 코드를 추가함(wait_data, arr_data[상태도데이터] 필요없어지게됨)




## 수정1차(정승진)
  - main 함수에서 if문에 wait큐가 비워져 있어도 돌아가는 버그 발생 --> if문에 wait가 비워져있으면 패스하는걸로 수정
  - bank.c Make Account 에서 같은 계좌 탐색하는것 추가
  - exit(0) 부재로 인해 함수가 두번도는 버그 해결


## 수정2차(정승진)
  - Make Account함수가 오버플로우 하던 문제 발생 --> rand 함수를 잘못 써준거였음 --> rand()% 형식으로 바꿔주고 해결
  - main함수에서 쓸데없는 while문이 돌아가는 버그 발생 --> while 조건문에 !is_empty(wait_3)의 부재로 인한 것이였음 --> 해결
