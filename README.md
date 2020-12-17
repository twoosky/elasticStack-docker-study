Elasticsearch
=================
* 사용자가 서버나 데이터베이스로부터 원하는 데이터를 실시간으로 수집하고 검색, 분석하여 시각화 시키는 오픈소스 서비스이다.
* Elastic Stack (Elastic Search + Logstash + Kibana + Filebeat)

## Elastic search
1. Full text search
 * 역 인덱스(Inverted Index) 구조로 데이터 저장
 
   * 추출된 각 키워드를 텀(term)이라고 부르며, term을 포함하는 도큐먼트들의 id를 바로 얻을 수 있음.
   
   * 역 인덱스가 가리키는 id의 배열값이 추가되는 것이므로 검색 속도 향상
   
     * 관계형 DB에서는 Text열을 한 줄씩 내려가면서 row 안의 내용을 모두 읽어 키워드를 찾아야 하므로 속도 저하
   
   <img src="https://gblobscdn.gitbook.com/assets%2F-Ln04DaYZaDjdiR_ZsKo%2F-LntL_BGpuFbNXy_sFtK%2F-LntLbibpXHABupWvXtu%2F6.1-03.png?alt=media&token=d2726f20-a7ea-4219-bcb0-340cbe1d21f1" width="500px" height="180px" title="px(픽셀) 크기 설정" alt="RubberDuck"></img><br/>
   
 * 텍스트 분석 (Text Analysis)
   
    * 문자열 필드가 저장될 때 데이터에서 검색어 토큰을 저장하기 위한 여러 단계의 처리 과정
      
 * 애널라이저 (Analyzer)
      
    * 텍스트 분석을 처리하는 기능
      
    * 0-3개의 캐릭터 필터(Character Filter)와 1개의 토크나이저(Tokenizer), 그리고 0-n개의 토큰 필터(Token Filter)로 이루어짐.
      
    * 캐릭터 필터 (Character Filter)
        
        * 입력된 텍스트 데이터를 필요에 따라 전체 문장에서 특정 문자를 대치하거나 제거하는 과정을 담당
          
    * 토크나이저 (Tokenizer)
          
        * 문장 내 단어들을 텀(term) 단위로 하나씩 분리하는 과정을 담당
        * 토크나이저는 반드시 1개만 적용 가능
          
    * 토큰 필터 (Token Filter)
        
        * 분리된 텀(term)들을 하나씩 가공하는 과정을 담당
        * 예시) lowercase : 토큰 필터를 이용해 대문자를 모두 소문자로 바꿔 대소문자 구별 없이 검색 가능
        
    * 불용어(stopword)
    
        * 검색어로서의 가치가 없는 단어 (a, an, are, at, i)
        
    * snowball 토큰 필터
    
        * 문법상 변형된 단어를 기본 형태로 변환하여 검색 가능하게 함
        * 예시) jumps와 jumping은 모두 jump로 변경 후 병합
        
    * synonym 토큰 필터
    
        * 동의어를 하나의 단어로 텀(term) 병합
        * 예시) quick 텀에 동의어로 fast를 지정하면 fast로 검색했을 때도 같은 의미인 quick을 포함하는 도큐먼트가 검색
    
2. Cluster
    
  * 가장 큰 시스템 단위를 의미하며, 최소 하나 이상의 노드로 이루어진 노드들의 집합
  
  * 전체 데이터를 함께 보유하고 모든 노드에서 연합 인덱싱 및 검색 기능 제공
  
  * 유일한이름으로 판별 (기본값 : 'elasticsearch')
  
  * 사용자는 클러스터를 대상으로 데이터를 저장하거나 검색 요청
  
  * 서로 다른 클러스터는 데이터의 접근, 교환을 할 수 없는 독립적인 시스템으로 유지
  
  * 대용량 데이터의 증가에 따른 스케일 아웃과 데이터 무결성을 유지하기 위한 클러스터링 지원
  
  * 클러스터를 기본으로 동작하며 1개의 노드만 있어도 클러스터 구성
  
  * 여러 노드가 하나의 클러스터로 묶이기 위해선 클러스터명(cluster.name) 설정이 모두 동일해야 함
    
  * 클러스터 구성
      
      * 여러 서버에 하나의 클러스터로 실행 / 하나의 서버에서 여러 클러스터 실행
      
          * http 포트 : 클라이언트와의 통신
          
          * tcp 포트 : 노드 간 데이터 교환
          
          * 일반적으로 1개의 물리 서버마다 하나의 노드 실행
          
          1. 3개의 서버에서 각 1개의 노드를 실행
          
             <img src="https://gblobscdn.gitbook.com/assets%2F-Ln04DaYZaDjdiR_ZsKo%2F-LnUvu8iUqn07h_vY2sX%2F-LnEXJ5Avdp4mVjPeH2b%2Fimage.png?alt=media&token=447df06c-a4d5-4330-81cb-2e71804db02a" width="300px" height="250px" title="px(픽셀) 크기 설정" alt="RubberDuck"></img><br/>
            
          2. 한 서버에서 2개의 노드 실행, 다른 서버에서 1개의 노드 실행
          
             <img src="https://gblobscdn.gitbook.com/assets%2F-Ln04DaYZaDjdiR_ZsKo%2F-LnUvu8iUqn07h_vY2sX%2F-LnEXWfNEyRQtfSEdK7v%2Fimage.png?alt=media&token=50451080-1637-4b44-a687-6de356a67115" width="300px" height="250px" title="px(픽셀) 크기 설정" alt="RubberDuck"></img><br/>
             
          3. 하나의 서버에서 서로 다른 2개의 클러스터 실행
          
             <img src="https://gblobscdn.gitbook.com/assets%2F-Ln04DaYZaDjdiR_ZsKo%2F-LnUvu8iUqn07h_vY2sX%2F-LnFBFFyMu6dOc9_OQ1s%2Fimage.png?alt=media&token=88af6fac-8297-4c79-a3e1-3f8779214559" width="300px" height="250px" title="px(픽셀) 크기 설정" alt="RubberDuck"></img><br/>
            
             * node-1과 node-2는 하나의 클러스터로 묶여있으므로 데이터 교환 가능
             * node-3은 클러스터가 다르므로 나머지 노드들과 데이터 교환 불가능
             * node-2는 node-1이 마스터로 있는 클러스터 es-cluster-2에 묶인 것
             * node-3는 자신이 es-cluster-2 클러스터의 마스터 노드
      
      * 디스커버리 (Discovery)
      
          * 노드가 처음 실행 될 때 같은 서버, 또는 'discovery.seed_hosts: [] '에 설정된 네트워크 상의 다른 노드들을 찾아 하나의 클러스터로 바인딩 하는 과정
          
          * 디스커버리 과정
          
              1. 'discovery.seed_hosts' 설정에 있는 주소 순서대로 노드가 있는지 여부 확인
                  * 노드가 존재하는 경우 > cluster.name 확인
                      * 일치하는 경우 > 같은 클러스터로 바인딩 > 종료
                      * 일치하지 않는 경우 > 1로 돌아가서 다음 주소 확인 반복
                  * 노드가 존재하지 않는 경우 > 1로 돌아가서 다음 주소 확인 반복
                  
              2. 주소가 끝날 때까지 노드를 찾지 못한 경우 > 스스로 새로운 클러스터 시작
              
               <img src="https://gblobscdn.gitbook.com/assets%2F-Ln04DaYZaDjdiR_ZsKo%2F-LnVEF1ED-x6P43UYEm-%2F-LnKbiLZhJ4PDthfVOko%2Fimage.png?alt=media&token=989ba2aa-5129-43db-b746-6738b6a121c1" width="300px" height="400px" title="px(픽셀) 크기 설정" alt="RubberDuck"></img><br/>
                
                
3. Node

    * 클러스터의 일부이며 데이터를 저장하고, 클러스터의 인덱싱 및 검색 기능에 참여하는 단일 서버
    
    * 노드에 할당되는 임의 UUID인 이름으로 식별
    
    * 클러스터 이름(cluster.name)을 통해 특정 클러스터와 결합하도록 노드 구성 가능
    
    * 역할에 따라 마스터노드, 데이터, ingest, client 노드로 사용
    
    * 노드 관련 속성
    
        * node.master : 마스터 기능 활성화 여부
        * node.data : 데이터 기능 활성화 여부
        * node.ingest : ingest 기능 활성화 여부
        * search.remote.connect : 외부 클러스터 접속 가능 여부
        
    * 노드 모드
    
        * Single Node mode
          
          * node.master:true / node.data:true / node.ingest:true / search.remote.connect:true
          * 모든 기능을 수행하는 모드, elasticsearch.yml 파일에 설정을 하지 않는다면 기본 설정으로 싱글모드 동작
          
        * Master Node mode
          
          * node.master: true / node.data:true / node.ingest:true / search.remote.connect:true
          * 클러스터의 제어를 담당하는 모드, 클러스터 전체를 관장하는 마스터 역할 수행
              
              * 인덱스 생성/변경/삭제 등의 역할 담당
              * 분산코디네이터 역할 담당하여 클러스터를 구성하는 노드의 상태를 주기적으로 점검하여 장애 대비
              * 마스터 노드는 클러스터에 다수(홀수개) 존재
              * 장애가 발생할 경우 후보 마스터 노드가 역할을 위임받아 클러스터 운영 유지
              
        * Data Node mode
        
          * node.master: false / node.data: true / node.ingest: false / search.remote.connect: false
          * 클러스터의 데이터를 보관하고 데이터의 CRUD (*CRUD = Creat(생성), Read(읽기), Update(갱신), Delete(삭제)), 검색, 집계 등 데이터 관련 작업을 담당하는 모드
          * 대용량 저장소, CRUD 작업과 검색, 집계 등의 역할 수행을 위한 전체적인 스펙을 갖춘 서버로 운영
          * 대용량 클러스터 환경에서 마스터 노드와 데이터 노드의 분리는 필수
          
        * Ingest Node mode
        
          * node.master: false / node.data: false / node.ingest: true / search.remote.connect: false
          * 다양한 형태의 데이터를 색인할 때 데이터의 전처리를 담당하는 모드
          * 데이터를 색인할 때 간단한 포맷 변경이나 유효성 검증 같은 전처리기가 필요할 때 해당 모드 이용
          
        * Coordination Node mode
        
          * node.master: false / node.data: false / node.ingest: false / search.remote.connect: false
          * 사용자 요청을 받아 처리하는 코디네이터 모드
          * 엘라스틱서치의 모든 노드는 기본적으로 코디네이션 모드 노드이다. 모든 노드가 사용자의 요청을 받아 처리할 수 있다.
          
          <img src="https://gblobscdn.gitbook.com/assets%2F-Ln04DaYZaDjdiR_ZsKo%2F-LnUyizASxjZepqPV1G1%2F-LnKqZrQzuaRIRdn6sLK%2Fimage.png?alt=media&token=46d874f2-f582-4339-8d0f-aef794a3ae93" width="400px" height="200px" title="px(픽셀) 크기 설정" alt="RubberDuck"></img><br/>
          
          * node-1이 마스터 노드, 별 표시로 구분
          
    * Split Brain
    
      * 네트워크 단절로 마스터 후보 노드가 분리되면 각자가 서로 다른 클러스터로 구성되어 동작, 이 상태에서 각자의 클러스터에 데이터 추가, 변경되고 나면 하나의 클러스터로 다시 합쳐졌을 때 데이터 정합성에 문제가 생기고 데이터 무결성이 유지될 수 없는 문제
      
      * 이를 방지하기 위해 마스터 후보 노드를 3개로 두고, 클러스터에 마스터 후보 노드가 최소 2개 이상 존재하는 클러스터만 동작하게 구현
     
      <img src="https://gblobscdn.gitbook.com/assets%2F-Ln04DaYZaDjdiR_ZsKo%2F-LnUyizASxjZepqPV1G1%2F-LnKxRWUMYG5ATpsysHP%2Fimage.png?alt=media&token=560fe149-3215-44d8-8f3c-c6383146c205" width="400px" height="200px" title="px(픽셀) 크기 설정" alt="RubberDuck"></img><br/>
      
      
4. Index

    * 도큐먼트(document)를 모아놓은 집합
    
      * document : 단일 데이터 단위
     
    * Indexing
      
      * 데이터가 검색될 수 있는 구조로 변경하기 위해 원본 문서를 검색어 토큰들로 변환하여 저장하는 과정
      
    * Search
      
      * 인덱스에 들어있는 검색어 토큰들을 포함하고 있는 문서를 찾아가는 과정
      
    * Query
    
      * 사용자가 원하는 문서를 찾거나 집계 결과를 출력하기 위해 검색 시 입력하는 검색어 또는 검색 조건
      
5. Shards

    * 데이터를 분산해서 저장하는 방법, Elasticsearch에서 스케일 아웃을 위해 index를 여러 shard로 쪼갬
    * 수평적으로 콘텐츠 볼륨을 split/scale 가능하다
    * 작업을 분산 및 병렬 처리 할 수 있으므로 성능/처리량 향상
    
    * 프라이머리 샤드(Primary Shard) / 복제본(Replica)
      
      * 클러스터가 Node에 있던 프라이머리 샤드를 유실하더라도 다른 노드에 복제본 샤드가 있으면 전체 데이터는 유실 없이 사용 가능
      * 클러스터는 유실된 노드의 복구 대기, 타임아웃이 지나 유실된 노드가 복구불가라고 판단되면 복제본 샤드의 복제 시작
      * 단일 노드의 경우 프라이머리 샤드만 존재, 데이터 가용성과 무결성을 위해 최소 3개 이상의 노드로 구성 권장
      * 복제본 샤드는 프라이머리 샤드와 동일한 노드에 할당되지 않는다.
      * 모든 복제본에서 검색을 병렬로 실행 가능하므로 검색 볼륨/처리량을 수평확장 할 수 있다.
      
    * 샤드의 크기가 미치는 영향
      
      * 장애 발생시 Elasticsearch가 데이터를 재배치할 때의 수행 속도는 네트워크와 디스크 성능 뿐만 아니라, 샤드의 크기와 개수에 따라 결정된다. 즉, 샤드의 크기가 크다면 장애시 클러스터의 복구능력에 좋지 않은 영향을 끼칠 수 있다.
      
6. RESTFul API

    * Rest API를 기본으로 지원하며 모든 데이터 조회, 입력, 삭제를 http 프로토콜을 통해 Rest API로 처리한다.
    
 ## Logstash

  * 다양한 소스(DB, csv파일 등)의 로그 또는 트랜잭션 데이터를 수집, 집계, 파싱하여 Elasticsearch로 전달
  
    * input    
    
      *입력을 사용하여 Logstash에 데이터 수집
    
    * Filter  
    
       * 형식이나 복잡성에 상관없이 설정을 통해 데이터를 동적으로 변환
          * grok : 구문 분석 및 임의의 텍스트로 구성
          * mutate : 이벤트 필드에서 일반적인 변환을 수행
          * drop : 이벤트를 삭제
          * clone : 이벤트의 복사본을 만듦
          * geoip : ip 주소의 지리적 위치에 대한 정보를 추가
    
    * output  
    
      *ElasticSearch, Email, ESC, Kafka등 원하는 저장소에 데이터를 전송
    
        * Elasticsearch : 수집한 데이터를 Elasticsearch에 전송
        * file : 디스크 파일에 기록
        * graphite : graphite에 전송 ( Graphite 란 메트릭을 저장하고 그래프로 작성하는데 사용되는 오픈 소스 도구)
        * stastsd : 카운터 및 타이머와 같은 통계를 수신하고 UDP를 통해 전송되며, 하나 이상의 플러그 가능한 백엔드 서비스에 집계를 보내는 서비스
        
    * codec
      
      * 입력 또는 출력의 일부로 작동 할 수 있는 스트림 필터
      * 대표적인 코덱에는 json, msgpack, plain이 있다.
      
## Kibana

  * Elasticsearch의 빠른 검색을 통해 데이터를 시각화 및 모니터링
  
## Filterbeats

  * 서버에 경량 에이전트로 설치되어 다양한 유형의 데이터를 Logstash 또는 ElasticSearch로 전송하는 오픈 솟 데이터 발송자
  * Filebeat, Metricbeat, Packetbeat, Winlogbeat, Heartbeat 등이 있으며 Libbeat을 이용하여 직접 구축도 가능
  
    * FileBeat
      * 서버에서 로그파일을 제공
      
    * PacketBeat
      * 응용 프로그램 서버 간에 교환되는 트랜잭션에 대한 정보를 제공하는 네트워크 패킷 분석기
      
    * MetricBeat
      * 서버에서 실행중인 운영 체제 및 서비스에서 Metrics를 주기적으로 수집하는 서버 모니터링 에이전트
      
    * WinlogBeat
      * Windows 이벤트 로그를 제공
      
 * Active-Standby
    * 평상 시에는 하나의 서버로 운영을 하고, 그와 스펙이 비슷한 서버를 예비로 준비한다. 메일서버는 OS영역과 Data영역이 물리적인 디스크가 분리되도록 관리한다. 예비서버는 OS가 설치된 형태로 언제라도 부팅하면 바로 사용 할 수 있는 상태로 준비, 장애가 발생되면 관리자가 판단하여 예비서버로 서비스 해야 한다고 판단이 되면 메일 서버에서 Data영역의 디스크를 분리하여 예비서버로 이전한 후 예비서버로 서비스 운영
    
    * 이 형태는 저렵한 비용으로 이중화 구성할 수 있지만, 시스템 장애 시간이 길고 안정성이 적은 방법이다.
    
 * Active-Active
    * 공유가 불가능하므로 사용자가 각 서버별로 고루 분산되도록 구성이 되고, 사용자는 자신이 속한 서버에서만 서비스를 이용할 수 있다.
    * 랜덤하게 서버에 접근한 후 사용자 인증 후 자신의 메일서버로 REDIRECT되는 형태
    * 장애가 발생하면 클러스터링 SW에 의해 감지가 되고, 자동으로 서비스 절체 및 이전이 이루어진다
      * 예시) HACMP : 서버#1에 장애가 발생하면 서버#1의 IP, DISK등의 정보가 서버 #2로 이전되어 서버#2에서 서버#1의 서비스를 대신 수행하므로서 시스템 장애 시간을 최소로 줄일 수 있다.
      
 ## Elasticsearch 데이터 처리
 
  * Elasticsearch는 데이터 저장 형식으로 json 도큐먼트를 사용
  
 1. REST API
 
   * http 프로토콜로 접근 가능
   * 고유 URL로 접근이 가능하며 http 메서드 PUT, POST, GET, DELETE를 이용해서 자원 처리  
     * RESTFul한 시스템    
   * curl 명령어, Kibana Dev Tools로 사용 가능
  
   * RESTFul한 시스템에서의 데이터 처리
  
     * 입력 : `PUT http://user.com/kim -d {"name":"kim", "age":38, "gender":"m"}`
     * 조회 : `GET http://user.com/kim`
     * 삭제 : `DELETE http://user.com/kim`
    
   * CRUD - 입력, 조회, 수정, 삭제
    
     * curl 명령어로 도큐먼트에 접근하는 URL
    
       `http://<호스트>:<포트>/<인덱스>/_doc/<도큐먼트 id>`
      
     * 입력 (PUT)
      
       `PUT <인덱스>/_doc/<도큐먼트 id>`
      
        * 처음으로 도큐먼트를 입력하면 결과에 '"result" : "created"'로 표시됨.
        * 동일한 URL에 다른 내용의 도큐먼트를 다시 입력하게 되면 새로운 도큐먼트로 덮어씌워짐 이 때 result에 updated로 표시됨.
      
       `PUT <인덱스>/_create/<도큐먼트 id>`
    
        * 기존 도큐먼트가 덮어씌워지는 것을 방지하기 위해 _doc 대신 _create로 새 도큐먼트 입력 
        * 이 때, 입력하려는 도큐먼트 id에 이미 데이터가 있는 경우 오류 발생
        
     * 조회 (GET)
     
       `GET <인덱스>/_doc/<도큐먼트 id>`
       
         * 문서의 내용이 _source 항목에 나타남 
         * 삭제되거나 입력되지 않은 도큐먼트를 GET하면 '"found" : false' 리턴
         * 삭제되거나 없는 인덱스의 도큐먼트를 GET하면 '"type" : "index_not_found_exception" , "status" : 404' 리턴
        
     * 삭제 (DELETE)
     
       `DELETE <인덱스>/_doc/<도큐먼트 id>`
       
         * my_index/_doc/1 도큐먼트 삭제, '"result" : "deleted"' 리턴
         
       `DELETE <인덱스>`
       
         * my_index 인덱스 전체 삭제
         * '"acknowledged" : true'  리턴

     * 수정 (POST)
     
       `POST <인덱스>/_doc`
       
         * PUT 메서드와 유사하게 데이터 입력에 사용 가능
         * <인덱스>/_doc까지만 입력하면 자동으로 임의의 도큐먼트 id 생성
       
       ```
       POST <인덱스>/_update/<도큐먼트 id>
       {
         "doc": {
           "필드 : 업데이트 내용"
         }
       }
       ```
       
         * 원하는 필드의 내용만 업데이트 가능
         * 업데이트 할 내용에 "doc" 지정자 사용
         
2. _bulk API

  * 여러 명령을 배치로 수행하기 위해 사용
  
  <img src="https://gblobscdn.gitbook.com/assets%2F-Ln04DaYZaDjdiR_ZsKo%2F-LnV0-25VfpscKAFEGLU%2F-LnV09PT6f3bcI4XKuga%2F4.3-01.png?alt=media&token=2df3f2e8-19a4-48a3-a480-d2bc99e2bdce" width="400px" height="200px" title="px(픽셀) 크기 설정" alt="RubberDuck"></img><br/>
  
  * 동일한 인덱스에서 수행되는 경우 `<인덱스명>/_bulk` 형식으로도 사용가능
  
    ```
    POST test/_bulk
    {"index" :{"_id":"1"}}
    {"filed":"value one"}
    ```
    
  * 파일에 저장 내용 실행
  
    * 벌크 명령을 파일로 저장하고 curl 명령으로 실행가능
    * 위의 내용을 bulk.json이라는 이름으로 파일 저장 후, `--data-binary`로 지정하면 저장된 파일로부터 입력할 명령과 데이터를 읽어올 수 있다.
    
3. 검색 API (_search API)

  * `GET <인덱스명>/_search` 형식으로 쿼리를 통한 검색 기능, 검색은 인덱스 단위로 이루어짐
  * 검색 시 쿼리를 넣지 않으면 해당 인덱스의 모든 도큐먼트 검색 match_all
  
  3-1. URL 검색
    
     * 요청 주소에 검색어를 넣어 검색하는 방식
     * _search 뒤에 q 파라메터를 사용해 검색어 입력
    
      `GET <인덱스>/_search?q=<검색어>` 
    
       * `hits.total.value` 에 검색 결과 전체에 해당하는 문서의 개수 표시되고 `hits:[]` 구문 안에 배열로 가장 정확도가 높은 문서 10개 나타남
       * AND, OR, NOT 사용 가능
       * 검색어를 특정 필드에서 찾고 싶으면 `q=<필드명>:<검색어> 형태로 입력
       
  3-2. 멀티테넌시(Multitenancy)
   
     * 여러 개의 인덱스를 한꺼번에 묶어서 검색할 수 있는 멀티테넌시
     `GET logs*/_search`
       
  3-3. QueryDSL
   
     * 풀 텍스트 쿼리 (Full Text Query)
     
       * match
         
         * match 쿼리를 이용하여 해당 인덱스의 해당 필드에 검색어가 포함되어 있는 모든 문서 검색
         * 여러 개의 검색어를 집어넣으면 OR 조건으로 검색이 되어 입력된 검색어 중 하나라도 포함된 모든 문서 검색
          ```
          GET <인덱스>/_search
          {
            "query": {
               "match": {
                  <필드명>:<검색어>
               }
          ...
          ```
          
          * 검색 조건을 AND로 바꾸려면 `<필드명>: {"query":<검색어>, "operator": }` 로 입력
         
       *  match_phrase
       
          * 구문을 검색하는 쿼리 
           
           ```
           GET <인덱스>/_search
          {
            "query": {
               "match_phrase": {
                  <필드명>:<검색어>
               }
          ...
           ```
          
           * `slop` 옵션을 통해 slop에 지정된 값만큼 단어 사이에 다른 검색어가 끼어드는 것을 허용할 수 있음
          
             `<필드명>: {"query":<검색어>, "slop": }`
          
       * query_string
       
         * URL검색에 사용하는 검색 문법을 본문 검색에 이용하고 싶을 때 사용
         
          ```
          GET <인덱스>/_search
          {
            "query": {
               "query_string": {
                  "default_filed": <필드명>,
                  "query": "(jumping AND lazy) OR |"quick dog|""
               }
          ...
           ```
         
     * Bool 복합 쿼리
     
       * 여러 쿼리를 조합하기 위해서 사용, 정확도를 위해 필요
         
         * must : 쿼리가 참인 도큐먼트들을 검색
         * must_not : 쿼리가 거짓인 도큐먼트들을 검색
         * should : 검색 결과 중 이 쿼리에 해당하는 도큐먼트의 점수를 높임
         * filter : 쿼리가 참인 도큐먼트를 검색하지만 스코어를 계산하지 않는다. must보다 검색 속도가 빠르고 캐싱이 가능하다
         
          ```
           GET <인덱스>/_search
          {
            "query": {
               "bool": {
                 "must": [
                  { <쿼리> }, ...
                 ],
                 ...
               }
          ...
           ```
           
     * 정확도 (Relevancy)
     
       * 검색 결과가 입력된 검색 조건과 얼마나 정확하게 일치하는 지
       
       * 스코어 점수
       
         * 검색 결과의 `_score` 항목에 높은 스코어 점수부터 나타남
         * BM25 알고리즘 이용
       
       * TF (Term Frequency)
       
         * 도큐먼트 내에 검색된 텀(term)을 많이 포함할수록 점수가 높아지는 것
         
       * IDF (Inverse Document Frequency)
       
         * 전체 인덱스에 검색한 텀을 포함하고 있는 도큐먼트 개수가 많을수록 그 텀의 자신의 점수가 감소하는 것 
         
            * ex) 전체 인덱스 내 quick이 들어간 문서는 3개, dog가 들어간 문서는 4개가 있다면, quick이 들어가 있는 결과가 점수가 높음
            
       * Field Length
         
         * 도큐먼트에서 필드 길이가 큰 필드보다는 짧은 필드에 있는 텀의 비중이 더 크다.
         
            * ex) 검색어가 제목과 내용 필드에 모두 있는 경우 텍스트 길이가 짧은 제목 필드에 텀이 들어있는 점수가 더 크다.
            
       * Bool : Should
       
         * 검색어 중 특정 검색어가 포함된 결과에 가중치를 줘서 상위로 올리고 싶은 경우 사용
            * match도 스코어 점수는 계산되지만, should는 기본 match 스코어 점수에 가중치를 줌 
         * match_phrase와 함께 유용하게 사용
         
     * 정확값 쿼리 (Exact Value Query)
     
       * 검색 조건의 참/거짓 여부만 판별해서 결과를 가져오는 것
       
       * bool : filter
       
         * 조건은 추가하지만 스코어에는 영향을 주지 않도록 제어할 때 사용
         * filter 내부에서 must_not과 같은 다른 bool 쿼리를 포함하려면 filter 내부에 bool 쿼리를 먼저 넣고 그 안에 다시 must_not을 넣어야 함
         
       * Keyword
       
         * 검색 문자열과 공백, 대소문자까지 정확히 일치하는 데이터만을 결과로 리턴
         
     * 범위 쿼리 (Range Query)
     
       * `range : { <필드명>: { <파라메터>:<값> } }` 으로 입력
       
       * range 쿼리 파라메터
       
          * gte (Greater-than or equal to) - 이상 (같거나 큼)
          * gt (Greater-than) – 초과 (큼)
          * lte (Less-than or equal to) - 이하 (같거나 작음)
          * lt (Less-than) - 미만 (작음)
         
            
            
       
           
           
           
          
           
          
         
         
       
     
     
     
    
 
  
  
  
  
  
         
         
       
    
  
  
  
      
      
   
