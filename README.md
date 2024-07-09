<h1> ethereum_whitepaper 정리 </h1>
ethereum white paper 원서 pdf : https://blockchainlab.com/pdf/Ethereum_white_paper-a_next_generation_smart_contract_and_decentralized_application_platform-vitalik-buterin.pdf</br>
ethereum white papaer 한글판 : https://ethereum.org/ko/whitepaper/</br>

<h2>history</h2>
<li>합의 알고리즘 : 분산된 시스템 내부에서 하나의 데이터 값에 대해 사람들의 동의를 구하는 과정</br></li>
<li>작업증명(PoW : Proof of work) : 풀기 어려운 문제를 가장 빨리 해결한 사람에게 블록을 생성할 수 있는 권한을 주고 보상으로 코인을 제공하는 방식</br></li>
<br/>
1998년 Wei Dai의 b-money는 탈중화 된 합의 및 계산 퍼즐을 해결함으로써 돈을 만들어내는 아이디어를 처음 제시했지만 탈중앙화를 가능하게 하는지에 대한 설명이 부족했음.</br>
2005년 Hal Finney는 재사용 가능한 작업증명이라는 개념을 소개했음. b-money와 Adam Back의 계산적으로 어려운 해시 캐시 퍼즐을 사용하여 암호화폐에 대한 개념을 만듦. 하지만 신뢰 가능한 컴퓨팅을 백엔드로 사용한다는 점에서 한계를 가졌음.</br>
2009년 사토시 나카모토는 공개키 암호화를 기반으로 하는 소유권 관리와 작업증명으로 알려진 소유자 추적을 위한 합의 알고리즘을 결합하여 최초로 탈중앙화 화폐를 구현함.</br>
<br/>
<h2>상태 전환 시스템으로써의 비트코인</h2>
<img src="https://ethereum.org/content/whitepaper/ethereum-state-transition.png"/>
<li>ledger : 거래를 기록하는 디지털 일지</br></li>
<li>UTXO : 20byte의 주소로 나타내어지는 암호 공개키로 소유자와 액면가 정보가 담긴 거래 정보.</br></li>
<li>민팅 : 블록체인에 등록됨</br></li>
</br>
암호화폐의 ledger는, 현존하는 모든 비트코인의 "상태"와 상태 및 거래를 입력으로 하고 결과를 출력으로 하는 '상태전환 함수'로 구성된 시스템임.</br>
이때 비트코인의 "상태"는 민팅 되었지만 아직 쓰이지 않았음을 의미함. 하나의 거래에서 입력은 현재 UTXO들 중 하나와 그 소유자 주소의 개인 키로부터 생성된 암호 서명을 포함하고, 출력은 "상태"에 추가 될 새로운 UTXO를 포함</br>
<h3>Ex) APPLY(S, TX) -> S'</h3>
<ol>
  <li>-1 참조된 UTXO가 기존 상태에 없으면 Error</br></li>
  <li>-2 암호서명이 소유자의 UTXO와 일치하지 않으면 Error</br></li>
  <li>모든 입력 UTXO의 액면가 총합이 모든 출력 UTXO의 액면가 총합보다 작으면 Error</br></li>
  <li>모든 입력 UTXO가 제거되고 모든 출력 UTXO가 추가된 새로운 상태 S'를 반환</br></br></li>  
</ol>
1-1은 트랜잭션 발송자가 없는 코인 사용을 방지, 1-2는 타인의 코인을 사용하는 것을 방지, 2는 가치보존 강제를 위함
</br></br>

<h2>채굴</h2>
<img src="https://ethereum.org/content/whitepaper/ethereum-blocks.png" />
비트코인의 탈중앙화 합의 프로세스는 네트워크 상의 노드들이 "블록"이라고 불리는 거래 패키지 생성을 지속적으로 시도함으로써 이루어짐. 각 블록은 타임스탬프, nonce, 이전 블록의 해시 정보, 이전 블록 이후 발생한 모든 거래 목록을 담게 됨.</br>
이러한 블록이 쌓이면서 비트 코인 ledger의 최신 상태를 나타내기 위해 지속적으로 업데이트 되는 <flex color="orange">블록체인</flex>이 만들어지게 됨.</br>
</br>
<h3>특정 블록 유효성 검증 알고리즘</h3>
<ol>
  <li>해당 블록이 참조하고 있는 앞선 블록이 실제하고 유효함</li>
  <li>타임스탬프는 앞선 블록의 타임스탬프 이후 시간, 차이는 최대 2시간</li>
  <li>해당 블록의 작업증명 유효</li>
  <li>S[0]은 이전 블록의 마지막 상태</li>
  <li>해당 블록이 가진 n개의 거래 리스트를 TX라고 할 때 모든 i(0 ~ n-1)에 대해 S[i+1] = APPLY(S[i], TX[i])를 적용. 어플리케이션이 오류 반환 시 종료하고 false를 return</li>
  <li>5번이 아닌 경우 true를 반환하고, S[n]은 블록 끝에 있는 상태로 등록</li>
</ol>
</br>
