Elasticsearch란?
=================
사용자가 서버나 데이터베이스로부터 원하는 데이터를 실시간으로 수집하고 검색, 분석하여 시각화 시키는 오픈소스 서비스이다.
기존의 ELK Solution (Elastic Search + Logstash + Kibana)에 Beats가 추가되면서 Elastic Stack 이라는 이름으로 서비스를 제공한다.

*Logstash
  * 다양한 소스(DB, csv파일 등)의 로그 또는 트랜잭션 데이터를 수집, 집계, 파싱하여 Elasticsearch로 전달
  * <img src="https://t1.daumcdn.net/cfile/tistory/999F343A5C51BB3018" width="150px" height="70px" title="px(픽셀) 크기 설정" alt="RubberDuck"></img><br/>
  
  * input
    * 입력을 사용하여 Logstash에 데이터 수집
    
  * Filter
    * 형식이나 복잡성에 상관없이 설정을 통해 데이터를 동적으로 변환
    1. grok : 구문 분석 및 임의의 텍스트로 구성
    2. mutate : 이벤트 필드에서 일반적인 변환을 수행
    3. drop : 이벤트를 삭제
    4. clone : 이벤트의 복사본을 만듦
    5. geoip : ip 주소의 지리적 위치에 대한 정보를 추가
    
  * output
    * ElasticSearch, Email, ESC, Kafka등 원하는 저장소에 데이터를 전송
    1. Elasticsearch : 수집한 데이터를 Elasticsearch에 전송
    2. file : 디스크 파일에 기록
    3. graphite : graphite에 전송 ( Graphite 란 메트릭을 저장하고 그래프로 작성하는데 사용되는 오픈 소스 도구)
    4. stastsd : 카운터 및 타이머와 같은 통계를 수신하고 UDP를 통해 전송되며, 하나 이상의 플러그 가능한 백엔드 서비스에 집계를 보내는 서비스
   
* Elasticsearch
  * Logstash로부터 받은 데이터를 검색 및 집계하여 필요한 관심 있는 정보 획득
  
* Kibana
  * Elasticsearch의 빠른 검색을 통해 데이터를 시각화 및 모니터링

* Beats
  * 데이터를 Logstash 또는 ElasticSearch로 전송
  
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile5.uf.tistory.com%2Fimage%2F993B7E495C98CAA7064E0B" width="200px" height="100px" title="px(픽셀) 크기 설정" alt="RubberDuck"></img><br/>

Elasticsearch 아키텍쳐 / 용어 정리
-------------------------------
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile27.uf.tistory.com%2Fimage%2F99A97A355C98D42D2E5196" width="200px" height="100px" title="px(픽셀) 크기 설정" alt="RubberDuck"></img><br/>

* 클러스터(cluster)
클러스터란 Elasticsearch에서 가장 큰 시스템 단위를 의미하며, 최소 하나 이상의 노드로 이루어진 노드들의 집합입니다. 서로 다른 클러스터는 데이터의 접근, 교환을 할 수 없는 독립적인 시스템으로 유지되며, 
여러 대의 서버가 하나의 클러스터를 구성할 수 있고, 한 서버에 여러 개의 클러스터가 존재할수도 있습니다.
    
* 노드(node)
Elasticsearch를 구성하는 하나의 단위 프로세스를 의미합니다.

  * master-eligible node 
  클러스터를 제어하는 마스터로 선택할 수 있는 노드
    * 인덱스 생성, 삭제
    * 클러스터 노드들의 추적, 관리
    * 데이터 입력 시 어느 샤드에 할당할 것인지
    
  * Data node
  데이터와 관련된 CRUD작업과 관련있는 노드
  
  * Ingest node
  데이터를 변환하는 등 사전 처리 파이프라인을 실행하는 역할
  
  * Coordination only node
  data node와 master-eligible node의 일을 대신하는 노드
  
* 인덱스(index) / 샤드(Shard) / 복제(Replica)
  * Index
  다소 유사한 특성을 갖는 문서들의 집합, 단일 클러스터에서 원하는만큼의 인덱스 정의
  
  * Shards
  데이터를 분산해서 저장하는 방법, Elasticsearch에서 스케일 아웃을 위해 index를 여러 shard로 쪼갬
    * 수평적으로 콘텐츠 볼륨을 split/scale 가능하다.
    * 작업을 분산 및 병렬 처리 할 수 있으므로 성능/처리량 향상
 
  * Replica
  노드를 손실했을 경우 데이터 신뢰성을 위해 샤드들을 복제한 것, replica는 서로 다른 노드에 존재할 것을 권장
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile29.uf.tistory.com%2Fimage%2F991563425C98CB341A49D4" width="150px" height="90px" title="px(픽셀) 크기 설정" alt="RubberDuck"></img><br/>

Elasticsearch 특징
------------------
* Scale out
샤드를 통해 규모가 수평적으로 늘어날 수 있음

* 고가용성
Replica를 통해 데이터의 안정성을 보장

* Schema Free
Json 문서를 통해 데이터 검색을 수행하므로 스키마 개념이 없음

* Restful
데이터 CURD작업은 HTTP Restful API를 통해 수행하며, 각각 다음과 같이 대응
|Data CRUD|Elasticsearch Restful|
|---|---|---|
|SELECT|GET|
|INSERT|PUT|
|UPDATE|POST|
|DELETE|DELETE|

    
    
    
    
    
    
    
    
    
    
    
    
