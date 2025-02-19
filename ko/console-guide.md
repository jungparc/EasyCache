## Database > EasyCache > 콘솔 사용 가이드

## 시작하기

EasyCache를 사용하려면 가장 먼저 복제 그룹을 생성해야 합니다.

## 복제 그룹

### 복제 그룹 생성

1. **Console > Database > EasyCache**의 **복제 그룹** 탭에서 **생성** 버튼을 누르면 **복제 그룹 생성** 창이 나타납니다.

![rep_001.PNG](https://static.toastoven.net/prod_easycache/21.06.04/rep_create_001.png)

2. 설정 창에서 표시된 필수 항목을 모두 입력하고 하단의 **생성** 버튼을 클릭합니다.

    * 복제 그룹 이름: 복제 그룹 이름을 입력합니다.
    * 설명: 복제 그룹의 설명을 입력합니다.
    * 서비스 포트: Redis의 포트 번호를 입력합니다.
        * 10000~12000 사이로 설정할 수 있습니다.
    * 버전: 생성할 Redis 버전을 선택합니다.
    * 인스턴스 타입: 복제 그룹의 사양을 선택합니다.
    * Max Memory: 최대 메모리를 조정해 동기화나 백업 실행 시 메모리 부족을 예방할 수 있습니다.
        * Redis 서버에 사용할 최대 메모리의 용량을 변경할 수 있습니다.
        * 필요할 때 관리용 메모리의 용량도 유연하게 확보할 수 있습니다.
    * 설정 프로필: Redis의 설정 파일을 선택합니다.
        * 기본 프로필을 제공합니다.
        * 설정 프로필을 추가해 선택할 수 있습니다.
    * VPC Subnet: 사설(private) 네트워크 통신을 원하는 Compute & Network 서비스의 서브넷을 선택합니다. 선택하지 않으면 기본 네트워크로 설정됩니다.
    * 비밀번호 설정: 비밀번호 셜정 여부를 선택합니다. 기본값은 '비밀번호 설정'입니다.
    * 자동 백업 설정: 자동 백업 사용 여부를 선택합니다.
        * 백업 보관 기간: 1일부터 최대 30일까지 보관할 수 있습니다.
        * 백업 시작 시간: 백업 시작 시간을 지정합니다. 30분 단위로 지정할 수 있습니다.
        * 백업 소요 시간: 백업 시작 시간부터 지정한 시간 사이의 임의의 시점에 시작합니다. 1시간부터 최대 3시간까지 지정할 수 있습니다.
3. **생성** 버튼을 클릭합니다.

4. 확인 화면에서 입력한 내용을 확인하고 **생성** 버튼을 클릭합니다.
   복제 그룹이 생성되면서 Master 노드가 생성됩니다. 생성될 때까지 몇 분 정도 걸립니다.
   
##### 제약 사항
* 서비스에 치명적인 영향을 줄 수 있는 명령어에 대해서 사용이 제한됩니다.
* 해당 명령어는 개발자 가이드를 참고 하세요. 

### 복제(노드 추가)

Redis가 지원하는 Replica 노드를 만들어 가용성을 높일 수 있습니다.

1. Replica 노드를 만들려면 원본 복제 그룹을 선택한 후 **노드 추가** 버튼을 클릭합니다.

![nod_ad_001.PNG](https://static.toastoven.net/prod_easycache/21.06.04/rep_node_add_002.png)

2. Master 노드가 다운되었는지 판단하기 위해 헬스 체크 응답 대기 시간을 설정할 수 있습니다. 기본값은 3000ms입니다.

3. Master 노드의 정보를 확인할 수 있습니다.

4. **추가** 버튼을 누르면 Replica 노드가 생성됩니다.
5. 생성된 노드의 정보는 **복제 그룹 > 노드 정보**에서 확인할 수 있습니다.
   생성 중 자동으로 복제 관계가 설정됩니다.

Replica 노드는 원본 Master 노드와 동일한 서버 사양입니다.
원본 Master 노드의 크기에 비례하여 Replica 노드 생성 시간이 늘어날 수 있습니다.

##### 제약 사항

* 원본 Master 노드에 최대 2대까지 Replica 노드를 만들 수 있습니다.
* 정상 상태가 아닌 Replica 노드가 존재하는 경우 먼저 삭제한 후 새로운 Replica 노드를 추가할 수 있습니다.
* Replica 노드의 Replica 노드는 만들 수 없습니다.

#### 고가용성(HA)

Standalone의 Master 노드에 Replica 노드를 추가하면 자동으로 고가용성 기능이 설정됩니다.

* Master 노드를 감시하여 장애가 발생했을 때 자동으로 장애 조치(failover)를 해 다운타임(downtime)을 최대한 단축할 수 있습니다.
* 장애 조치(failover)는 장애가 발생한 Master 노드를 감지해 자동으로 Replica 노드를 Master 노드로 승격시키는 것을 말합니다.
* Master, Replica 노드의 장애 및 상태에 관한 이벤트를 확인할 수 있습니다.

##### 제약 사항

* Replica 노드 1대 추가 시 HA 설정에 실패하면 **복제 그룹 > 기본 정보**에서 **HA 재설정** 버튼을 클릭해 HA를 다시 설정할 수 있습니다.

![rep_ha_error_001.PNG](https://static.toastoven.net/prod_easycache/20.07.09/rep_ha_error_001.PNG)

* Replica 노드 2대 추가 시 HA 설정 갱신에 실패하면 **복제 그룹 > 기본 정보**에서 **HA 설정 갱신** 버튼을 클릭해 HA 설정 갱신을 다시 수행할 수 있습니다

* 장애가 발생해 장애 조치를 한 경우, 장애가 발생한 기존 Master 노드는 중지됩니다. 장애가 발생한 노드를 삭제하면 고가용성 기능을 사용하지 않는 일반 Standalone의 Master 노드로 변경됩니다.
* Standalone이 된 Master 노드에 Replica 노드를 추가하면 고가용성 기능을 새로 지정해 사용할 수 있습니다.
* 변경된 새 Master 노드는 기존 Master 노드의 접속에 사용되는 도메인을 승계합니다.
* 장애 조치를 수행한 기존의 Master 노드는 ‘이용 불가’ 상태가 되고, 이용 불가 상태에서 새 마스터 노드로만 고가용성 기능이 제공되지 않습니다.

### 복제 그룹 수정

1. 원본 복제 그룹을 선택하고 **수정** 버튼을 클릭합니다.
2. **복제 그룹 수정** 대화 상자에서 이름과 백업 기간 등을 설정합니다.

![rep_mo_001.PNG](https://static.toastoven.net/prod_easycache/20.07.09/rep_modify_002.PNG)

* 복제 그룹 이름: 복제 그룹 이름을 변경할 수 있습니다.
* 설명: 복제 그룹 설명을 변경할 수 있습니다.
* 설정 프로필: Redis 설정을 변경할 수 있습니다.
* Max Memory: 사용할 최대 메모리의 용량을 변경할 수 있습니다.

* 자동 백업 설정: 자동 백업 사용 여부를 선택합니다.
    * 보관 기간: 1일부터 최대 30일까지 보관할 수 있습니다.
    * 백업 시작 시간: 백업 시작 시각을 지정합니다. 30분 단위로 지정할 수 있습니다.
    * 백업 소요 시간: 백업 시작 시각부터 지정한 시간 사이의 임의의 시점에 시작합니다. 1시간부터 최대 3시간까지 지정할 수 있습니다.

- Replica 노드가 있을 때 확인할 수 있는 항목은 아래와 같습니다.
  - 마스터 다운 판정 시간: Master 노드가 다운되었는지 판단하기 위해 헬스 체크 응답 대기 시간을 설정할 수 있습니다. 기본값은 3000ms입니다.

3. 변경 내용을 확인하고 **변경** 버튼을 클릭합니다.
    한번 설정한 서비스 포트, Redis 버전, 인스턴스 타입, 가용성 영역은 변경할 수 없습니다.
### 자동 백업

* 매일 1회 지정된 시간에 자동으로 메모리 데이터(RDB 파일)를 백업합니다.
* 생성된 자동 백업은 **백업 **탭에서 관리할 수 있습니다.
* 백업 대상이 된 복제 그룹이 삭제되면 백업 파일도 삭제됩니다.
* 지정된 백업 보관 기간이 지나면 백업 파일은 자동으로 삭제됩니다.
* 자동 백업은 백업 시작 시각부터 백업 소요 시간 사이 중 임의의 시점에 시작됩니다.

### 수동 백업 생성

복제 그룹의 백업을 원하는 시점에 바로 생성할 수 있습니다. 백업 대상이 된 복제 그룹을 삭제해도 수동 백업 파일은 삭제되지 않습니다.

* 생성된 수동 백업은 **백업**탭에서 관리할 수 있습니다.
* 백업이 실행되는 동안 성능이 저하될 수 있습니다.
* 설정된 백업 보관 기간이 지나면 백업 파일은 자동으로 삭제됩니다.
* 백업 대상인 복제 그룹이 삭제되었다면 기본 정보에서 복제 그룹의 상세 내용은 표시되지 않습니다.

1. 수동 백업 파일을 만들려면  대상 복제 그룹을 선택한 후 **수동 백업** 버튼을 클릭합니다.
2. **수동 백업** 대화 상자에서 정보를 입력하고 **백업** 버튼을 클릭합니다. 
    데이터 크기에 비례해 백업 생성 시간이 늘어날 수 있습니다.
    ![manual_backup_001.png](https://static.toastoven.net/prod_easycache/20.07.09/rep_manual_backup_001.PNG)

* 백업 이름: 백업 이름을 입력합니다.
* 설명: 백업 설명을 입력합니다.
* 백업 보관 기간: 삭제하지 않거나 1일부터 최대 30일까지 보관할 수 있습니다.

### 공인 도메인 설정
* 복제 그룹은 같은 서브넷을 사용하는 인스턴스에서만 접속할 수 있습니다만, 외부에서 접속을 하고 싶다면 공인 도메인 설정을 할 수 있습니다.

![manual_backup_001.png](https://static.toastoven.net/prod_easycache/20.07.09/rep_public_domain_001.png)

### 읽기 전용 도메인 설정
* 읽기 전용 도메인을 설정하려면, Replica 노드가 추가된 대상 복제 그룹을 선택한 후 기타 액션 버튼(⋯)을 클릭하고 **읽기 전용 도메인 설정**을 클릭합니다.
* 설정된 읽기 전용 도메인은 복제 그룹을 생성할 때 선택한 VPC서브넷에서 접속가능한 사설도메인으로 Replica 노드의 IP가 바인딩됩니다.
* 설정된 읽기 전용 도메인은 **복제 그룹 > 접속 정보**에서 확인할 수 있습니다.
* Master 노드의 장애로 장애 조치가 발생하는 경우
    * Replica 노드가 1대인 경우
        * Replica 노드로 변경된 구 Master 노드를 복구하거나 수동으로 삭제한 후 새 Replica 노드를 추가하기 전까지 읽기 전용 도메인은 장애 조치로 Master 노드로 승격된 구 Replica 노드의 IP가 부여된 상태가 유지됩니다.
        * Replica 노드로 변경된 구 Master 노드를 복구하거나 수동으로 삭제한 후 새 Replica 노드를 추가하는 경우, 읽기 전용 도메인은 새 Replica 노드의 IP로 바인딩이 변경됩니다.
            * 바인딩 변경에 실패하면 복제 그룹 > 접속 정보에서 수동으로 재시도할 수 있습니다.
    * Replica 노드가 2대인 경우
        * 새로운 Master 노드로 승격된 구 Replica 노드의 IP는 바인딩에서 제외됩니다
        * Replica 노드로 변경된 구 Master 노드를 복구하거나 수동으로 삭제한 후 새 Replica 노드를 추가하는 경우, 읽기 전용 도메인에 새 Replica 노드의 IP가 추가됩니다.

* Replica 노드에 장애가 발생하거나 Replica 노드를 삭제하는 경우
    * Replica 노드가 1대인 경우
        * 읽기 전용 도메인은 Master 노드의 IP로 바인딩이 변경됩니다.
        * Replica 노드를 복구하거나 수동으로 삭제한 후 새 Replica 노드를 추가하는 경우, 읽기 전용 도메인은 새 Replica 노드의 IP로 바인딩이 변경됩니다.
            * 바인딩 변경에 실패하면 복제 그룹 > 접속 정보에서 수동으로 재시도할 수 있습니다.
    * Replica 노드가 2대인 경우
        * 읽기 전용 도메인에서 해당 Replica 노드의 IP는 제외됩니다.
        * Replica 노드를 복구하거나 수동으로 삭제한 후 새 Replica 노드를 추가하는 경우, 해당 Replica 노드의 IP가 읽기 전용 도메인의 바인딩에 추가됩니다.
    
* 읽기 전용 도메인을 설정한 상태에서 복제 그룹을 삭제하거나 서비스를 비활성화하는 경우 읽기 전용 도메인은 해제됩니다.

##### 제약 사항
* 읽기 전용 도메인의 바인딩이 변경되는 경우
    * Master 노드에 장애가 발생해 장애 조치를 한 후 새 Replica 노드를 추가하는 경우 등, 접속 중단 없이 바인딩이 변경되는 경우
        * 읽기 전용 도메인으로 접속한 클라이언트에서 도메인의 바인딩 변경을 감지하는 기능을 지원하거나 해당 로직이 구현되어 있는 경우에는 변경된 바인딩 IP로 재접속되나, 그렇지 않은 경우 기존 접속을 중단하고 다시 접속해야 합니다.
    * Replica 노드에 장애가 발생하거나 Replica 노드가 삭제되는 경우처럼 바인딩 변경 시 접속 중단이 동반되는 경우
        * 클라이언트에서 자동 재접속 기능을 지원하거나 해당 로직이 구현 되어 있는 경우에는 변경된 바인딩 IP로 재접속되나, 그렇지 않은 경우 다시 접속해야합니다.

### 데이터 가져오기
* 복제 그룹을 선택한 후 기타 액션 버튼(⋯)을 클릭하고 **데이터 가져오기**를 클릭해 데이터를 가져올 수 있습니다.

![data_import_001.png](https://static.toastoven.net/prod_easycache/21.07.07/data_import_001.png)

* 유저의 오브젝트 스토리지에 있는 RDB 파일을 사용 중인 노드로 가져올 수 있습니다. 이때 오브젝트 스토리지와 EasyCache의 리전은 같아야 합니다.
* 데이터를 가져올 때, 오브젝트 스토리지의 API 엔드포인트 설정, 컨테이너와 RDB 파일 경로가 필요합니다. 
* 컨테이너와 RDB 파일 경로에는 영어 대소문자, 숫자, 특수기호 하이픈(-), 밑줄(_), 빗금(/), 마침표(.)만 입력할 수 있습니다.
* **데이터 가져오기** 후, 노드의 이전 데이터는 삭제되므로 가져오기 실행 전 백업을 권장합니다. 
* **데이터 가져오기** 진행 중에는 노드를 이용할 수 없습니다. 진행 상황은 이벤트에서 확인할 수 있습니다. 
* EasyCache Redis와 같은 버전 또는 이전 버전에서 만들어진 RDB 파일만 가져올 수 있습니다. EasyCache Redis보다 새로운 버전에서 만들어진 RDB 파일은 가져올 수 없습니다.

### 데이터 내보내기
* 데이터를 내보내려면 복제 그룹을 선택한 후 기타 액션 버튼(⋯)을 클릭하고 **데이터 내보내기**를 클릭합니다.

![data_export_001.png](https://static.toastoven.net/prod_easycache/21.07.07/data_export_001.png)

* EasyCache 복제 그룹의 데이터를 유저의 오브젝트 스토리지로 내보낼 수 있습니다. 이때 오브젝트 스토리지와 EasyCache의 리전은 같아야 합니다.
* 데이터를 내보낼 때, 오브젝트 스토리지의 API 엔드포인트 설정, 데이터를 내보낼 컨테이너 경로, 파일 이름 작성에 필요한 Prefix를 입력합니다. 
* 컨테이너 경로와 Prefix에는 영어 대소문자, 숫자, 특수기호 하이픈(-), 밑줄(_), 빗금(/), 마침표(.)만 입력할 수 있습니다.
* EasyCache 복제 그룹의 데이터가 1GB 이상일 경우 오브젝트 스토리지로 내보낼 때 세그먼트 오브젝트가 자동으로 만들어집니다.

### 인스턴스 타입 변경 

* 사용 중인 노드의 인스턴스 타입을 변경할 수 있습니다. 
* 인스턴스 타입은 현재 노드보다 사양이 높은 인스턴스로만 변경할 수 있습니다.
* 인스턴스 타입 변경 중에 노드는 잠시 정지됩니다. 
* 복제 그룹이 Standalone일 경우 인스턴스 타입을 변경하면 백업 시점의 데이터로 돌아가며, 백업을 하지 않으면 데이터가 초기화됩니다.
* Replica 노드가 있을 경우 Master 노드의 인스턴스 타입 변경을 위해 장애 조치(failover)가 발생합니다.

![instance_type_001.png](https://static.toastoven.net/prod_easycache/20.07.09/rep_instance_type_001.png)

##### 제약 사항

* 수동 백업 생성 중에 중복으로 수동 백업을 할 수 없습니다. 진행 중인 수동 백업이 끝난 뒤 다시 시도해 주세요.
* 자동 백업 시간에 수동 백업을 할 경우 수동 백업이 즉시 생성되지 않고 지연될 수 있습니다.

### 버전 업그레이드
* Redis 버전 5인 기존 복제 그룹을 Redis 버전 6으로 업그레이드할 수 있습니다.
* 버전 업그레이드를 실행하려면 Redis 버전 5인 대상 복제 그룹을 선택한 후 기타 액션 버튼(⋯)을 클릭하고 **버전 업그레이드**를 클릭합니다.
* **버전 업그레이드** 대화 상자에서 버전 업그레이드 시 적용될 설정 프로필을 선택할 수 있습니다.
* 버전 업그레이드를 실행하기 전에 데이터 백업을 수행하면 데이터 유실 등 만일의 상황에 대비할 수 있습니다.
* 복제 그룹의 네트워크 트래픽이 적을 때 버전 업그레이드를 실행하면 업그레이드 속도 및 안정성을 높일 수 있습니다.

![version_up_001.png](https://static.toastoven.net/prod_easycache/21.10.29/version_up_001.png)

##### 제약 사항
* Redis 버전을 업그레이드하려는 기존 복제 그룹에 기본 설정 프로필 이외의 사용자 설정 프로필이 적용되어 있는 경우, 업그레이드 시 동일한 설정 프로필을 적용하려면 **프로필 설정**에서 Redis 버전 6인 사용자 설정 프로필을 미리 생성한 후 버전 업그레이드 시 해당 설정 프로필을 선택해야 합니다.
* 읽기 전용 도메인이 설정되어 있는 경우는 읽기 전용 도메인 설정을 해제한 후 버전 업그레이드를 할 수 있습니다.
* **standalone** 타입의 복제 그룹의 경우
    * 버전 업그레이드 도중에 발생하는 복제 그룹에 대한 쓰기는 업그레이드 이후의 데이터 복원 대상에서 제외됩니다.
    * 버전 업그레이드 도중에 Redis 서버가 재시작되면 일시적으로 복제 그룹에 대한 읽기 및 쓰기를 할 수 없습니다.
* **replication** 타입의 복제 그룹의 경우
    * 버전 업그레이드 후 Master 노드가 변경됩니다.
    * 버전 업그레이드 도중에 업그레이드에 필요한 마스터 변경이 수행되는 동안 일시적으로 복제 그룹에 대한 읽기 및 쓰기를 할 수 없으며, 장애 조치(failover) 이벤트가 발생할 수 있습니다.
    * 버전 업그레이드 도중에 HA 노드가 업그레이드되는 동안에는 장애 조치가 보장되지 않습니다.
* 버전 업그레이드에 실패하면 **복제 그룹 > ⋯ > 버전 업그레이드**를 클릭해서 버전 업그레이드를 다시 실행할 수 있습니다. 

### 마스터 변경

* Master 노드를 변경하려면 Replica 노드가 추가된 대상 복제 그룹을 선택한 후 기타 액션 버튼(⋯)을 클릭하고 **마스터 변경**을 클릭합니다.
* Replica 노드는 Master 노드로 변경되고 기존 Master 노드는 Replica 노드로 변경됩니다.
* Replica 노드가 2대인 경우, 시스템에서 적절한 Replica 노드를 Master 노드로 변경합니다.

### 복제 그룹 상세

복제 그룹의 기본, 접속, 노드, 모니터링 등의 상세 정보를 확인할 수 있습니다.
#### 기본 정보

생성된 복제 그룹을 선택하고 **기본 정보** 탭을 누르면 복제 그룹의 상세 정보를 확인할 수 있습니다.

![rep_detail_001.PNG](https://static.toastoven.net/prod_easycache/21.06.04/rep_detail_002.png)

확인할 수 있는 항목은 다음과 같습니다.

* 복제 그룹 이름, 설명, 타입, 버전, 서비스 포트, 인스턴스 타입
* Max Memory(최대 메모리), 가용성 영역, 설정 프로필
* VPC Subnet(서브넷), 생성일, 자동 백업 설정, 노드 수

Replica 노드가 있을 경우에 확인할 수 있는 항목은 아래와 같습니다.

* 마스터 다운 판정 시간

#### 복제 그룹 접속

생성된 복제 그룹을 선택하고 **접속 정보** 탭을 누릅니다.

![rep_de_002.PNG](https://static.toastoven.net/prod_easycache/20.08.07/rep_connection_info_kr.png)

* 암호화된 비밀번호를 보려면 **보기** 버튼을 클릭합니다.
* **복사**버튼을 누르면 비밀번호를 복사할 수 있습니다.
* 접속 가능한 도메인 정보를 확인할 수 있습니다.
* 공인 도메인을 설정하지 않은 Redis 노드는 외부에서 접근할 수 없습니다.
* **복사**버튼을 누르면 도메인을 복사할 수 있습니다.
* 접속 정보는 같은 VPC Subnet으로 연결된 노드의 애플리케이션에서 사용할 수 있습니다.
* 커맨드는 같은 VPC Subnet으로 연결된 노드에서 실행할 수 있습니다.
* 접속 제어 정보: 복제 그룹에 접근 가능한 사용자를 CIDR 형식으로 입력합니다.
    * **내 IP 표시**: 로컬 IP가 CIDR 형식으로 표시됩니다.
    * **생성** 버튼을 누르면 등록됩니다.
    * 접속 제어 정보에 등록되지 않은 IP는 접속할 수 없습니다.

#### 노드 정보

생성된 복제 그룹을 선택하고 **노드 정보** 탭을 누르면 복제 그룹 노드의 상세 정보와 로그를 확인할 수 있습니다.

![rep_node_info_001.PNG](https://static.toastoven.net/prod_easycache/21.07.07/rep_node_info_002.png)

* 확인할 수 있는 항목은 다음과 같습니다.
    * 노드 이름, 종류, IP, 가용성 영역, 생성일, 상태
* 노드의 로그를 보려면 **로그 보기** 버튼을 클릭합니다.

##### 로그 보기

각 노드에서 최대 1개월간의 로그를 검색할 수 있습니다. 

![node_log_view_001.png](https://static.toastoven.net/prod_easycache/20.10.30/node_log_view_001.png)

* **1시간**, **24시간**, **1주**, **지정** 버튼을 클릭해 검색 기간을 변경할 수 있습니다. 
* **지정** 버튼을 클릭하면 나타나는 캘린더에서 원하는 검색 기간을 지정할 수 있습니다. 
* **현재 시간** 버튼을 클릭하면 현재 시간을 기준으로 선택한 검색 기간을 재검색합니다.
* **현재 시간** 버튼의 오른쪽에 있는 화살표 버튼을 클릭하면 검색 기간 만큼의 이전 시간, 이후 시간을 검색할 수 있습니다.
* **전체 화면 보기** 버튼을 클릭하면 새 창에서 1개월간의 모든 로그를 확인할 수 있습니다.

## 모니터링

EasyCache는 Redis 운영 및 사용에 필요한 모니터링 항목을 1분 마다 수집하고 있으며, 수집한 데이터를 차트로 보여줍니다.  

![monitoring_001.png](https://static.toastoven.net/prod_easycache/20.05.14/monitoring_001.PNG)

* 1시간, 24시간 등의 버튼을 누를 때마다 현재 시각을 기준으로 계산하여 갱신합니다.
    * **1시간** 버튼은 1분마다 수집한 데이터를 차트에 표시합니다.
    * **12시간** 버튼은 수집한 데이터의 10분간의 평균값을 차트에 표시합니다.
    * **24시간** 버튼은 수집한 데이터의 10분간의 평균값을 차트에 표시합니다.
    * **1개월** 버튼은 수집한 데이터의 6시간의 평균값을 차트에 표시합니다.
    * **지정** 버튼을 클릭해 직접 검색 기간을 지정할 수 있습니다.
* 캘린터를 클릭하여 검색 시점을 지정할 수 있습니다.
    * 캘린더에서 날짜나 시간을 선택하여도 선택한 검색 기간은 유지됩니다.
* 현재 시간 버튼을 클릭하면 현재시간을 기준으로 선택한 검색 기간을 재검색 합니다.
* 현재 시간 버튼의 오른쪽에 있는 화살표 버튼을 이용하여 검색 기간 만큼의 이전 시간, 이후 시간을 검색할 수 있습니다.
* 복제 그룹 드롭다운에서 차트를 표시할 복제 그룹을 선택할 수 있습니다.
* 자동 갱신 을 체크하면 60초 마다 차트 데이터를 갱신합니다.
* 차트를 클릭하면 차트를 확대하여 표시할 수 있습니다.
* 확대한 차트에서는 통계와 집계 기간을 변경하여 표시할 수 있습니다.
    * 통계 방법은 합산 데이터를 표시할 경우 사용되며 집계 기간이 1분이 경우에는 로우 데이터를 사용하므로 통계를 변경하여도 같은 값을 표시하게 됩니다.
* 모니터링 데이터 보존 기간은 40일입니다.

![monitoring_002.PNG](https://static.toastoven.net/prod_easycache/20.05.14/monitoring_002.PNG)
* 모니터링 항목은 **필터 조건**에서 원하는 항목만을 표시하도록 선택할 수 있습니다.
* 모니터링 항목은 다음과 같습니다.
    * CPU 이용률
    * 시스템 메모리
    * 연결된 클라이언트
    * 블록된 클라이언트
    * Redis 메모리 사용량
    * Redis 메모리 사용량(rss)
    * 메모리 파편화 비율
    * 초당 처리한 명령 수
    * 입력 바이트
    * 출력 바이트
    * 만료된 키 수(expired)
    * 삭제된 키 수(evicted)
    * 조회 성공 수
    * 조회 실패 수
    * 조회 성공률
    * 키 개수
    * get 실행 횟수
    * get usec/get calls
    * set 실행 횟수
    * set usec/get calls


## 백업

**백업** 탭에서 백업과 백업 삭제 등을 할 수 있습니다. 백업 중에는 성능이 저하될 수 있으므로 서비스 부하가 적은 시간에 백업하는 것이 좋습니다.

![backup_001.PNG](https://static.toastoven.net/prod_easycache/20.04.28/backup_001.PNG)

* 백업 파일을 하나 또는 여러 개 선택해 삭제할 수 있습니다.
* 검색어란에 백업 이름 또는 복제 그룹 이름을 입력하고 **검색**을 누르면 결과가 나타납니다.
* **새로 고침**을 눌러 백업 파일 목록을 갱신해 정보를 확인할 수 있습니다.

* **기본 정보**에서 백업 파일 상세 내용과 복제 그룹의 상세 내용을 확인할 수 있습니다.
![backup_002.PNG](https://static.toastoven.net/prod_easycache/20.04.28/backup_003.PNG)
    * 백업 파일 상세
        * 백업 이름, 설명, 타입, 캐시 크기, 백업 파일 크기, 백업 보관 기간, 백업 최종 보관일, 상태, 백업 시작 일자
    * 복제 그룹 상세
        * 복제 그룹 이름, 인스턴스 타입, 버전, Max Memory(최대 메모리), 서비스 포트, VPC Subnet

### 복원

보관된 백업 파일을 이용해 메모리 데이터를 복원할 수 있습니다. 

#### 신규 복제 그룹 복원

* 복원하려면 백업 파일을 선택하고 **신규 복제 그룹 복원**을 클릭합니다. 복원 시 원본 노드를 변경하지 않고 같은 사양 또는 다른 사양의 새 노드를 생성할 수 있습니다.
  ![restore_001.PNG](https://static.toastoven.net/prod_easycache/21.08.02/restore_001.png)

* **신규 복제 그룹 복원** 대화 상자에서 다음 항목을 입력하고 **생성** 버튼을 클릭합니다. 생성된 복제 그룹은 **복제 그룹** 탭에서 확인할 수 있습니다.
    * 백업 이름: 복원할 백업 파일 이름
    * 복제 그룹 이름: 복제 그룹 이름을 입력합니다.
    * 설명: 복제 그룹의 설명을 입력합니다.
    * 서비스 포트: 백업 대상이 된 복제 그룹의 포트가 표시됩니다.
        * Redis의 포트 번호를 변경할 수 있습니다.
        * 10000~12000 사이로 설정할 수 있습니다.
    * 버전: 백업 대상이 된 복제 그룹의 Redis 버전이 표시됩니다.
    * 인스턴스 타입: 백업 대상이 된 복제 그룹의 사양이 표시됩니다.
        * 백업의 캐시 크기보다 큰 인스턴트 타입만 표시됩니다.
        * 인스턴트 타입을 변경할 수 있습니다.
    * Max Memory: 최대 메모리를 조정해 동기화나 백업 실행 시 메모리 부족을 예방할 수 있습니다.
        * Redis 서버에 사용할 최대 메모리의 용량을 변경할 수 있습니다.
        * 최대 메모리 용량을 변경할 수 있어 필요할 때 관리용 메모리의 용량도 유연하게 확보할 수 있습니다.
    * 설정 프로필: 백업 대상이 된 복제 그룹의 Redis 설정 파일이 표시됩니다.
        * 설정 프로필을 추가해 변경할 수 있습니다.
    * VPC Subnet: 백업 대상이 된 복제 그룹의 VPC Subnet이 표시됩니다.
        * 사설(private) 네트워크 통신을 원하는 Compute & Network 서비스의 서브넷을 선택할 수 있습니다.
    * 자동 백업 설정: 자동 백업 사용 여부를 선택합니다.
        * 백업 보관 기간: 1일부터 최대 30일까지 보관할 수 있습니다.
        * 백업 시작 시간: 백업 시작 시각을 지정합니다. 30분 단위로 지정할 수 있습니다.
        * 백업 지연 시간: 백업 시작 시각부터 지정한 시간 사이의 임의의 시점에 시작합니다. 3시간까지 지정할 수 있습니다.

#### 기존 복제 그룹 복원

* 복원하려면 백업 파일을 선택하고 **기존 복제 그룹 복원**을 클릭합니다. 선택한 복제 그룹의 데이터를 백업 데이터로 변경합니다. 
  ![restore_002.PNG](https://static.toastoven.net/prod_easycache/21.08.02/restore_002.png)

* 복제 그룹의 복원 중에는 노드를 사용할 수 없으며, 기존 데이터는 삭제됩니다. 진행 상황은 이벤트에서 확인할 수 있습니다. 
* 이용 불가 상태의 복제 그룹은 선택할 수 없으며, 복제 그룹의 maxmemory가 백업의 캐시 사이즈보다 커야 합니다.
* 백업이 만들어진 복제 그룹의 버전과 동일하거나, 버전이 높은 복제 그룹에만 복원할 수 있습니다.

## 설정 프로필

### 설정 프로필 생성

변경이 가능한 Redis의 설정을 프로필 형태로 등록해 관리할 수 있습니다.

1. 프로필로 등록하려면 **프로필 설정** 탭에서 **프로필 생성** 버튼을 클릭합니다.
2. **프로필 생성** 대화 상자에서 프로필 이름, 설명, 프로필 적용 버전을 선택할 수 있습니다. 

![pro_002.PNG](https://static.toastoven.net/prod_easycache/20.05.14/profile_001_ko.png)

3. **상세 설정**을 클릭하여 프로필 설정을 항목 별로 변경 할 수 있습니다. 변경 없이 프로필을 등록하면 기본 설정을 사용합니다.

![pro_003.PNG](https://static.toastoven.net/prod_easycache/20.05.14/profile_003_ko.png)

4. **생성** 버튼을 클릭하여 프로필을 등록할 수 있습니다. 

    * 등록한 프로필 정보를 수정하면 이용 중인 노드에도 반영됩니다.

    * 등록한 프로필을 삭제할 수 있습니다. 단, 이용 중인 노드가 있는 프로필은 삭제할 수 없습니다.

    * 등록한 프로필을 복사하여 사용할 수 있습니다. 또한 복사할때 항목 값을 변경할 수 있습니다.

    * 기본 설정 정보가 있는 기본 프로필을 제공합니다.

    * 기본 프로필은 수정, 삭제할 수 없습니다.

    * 프로필의 상태를 확인할 수 있습니다.

| 상태         | 설명                                                         |
| ------------ | ------------------------------------------------------------ |
| 정상         | 프로필을 수정, 삭제할 수 있습니다.                           |
| 변경 적용 중 | 프로필을 변경했고 변경 내용을 각 노드에 전파 중인 상태입니다. <br />변경 내용의 전파가 끝나면 상태는 정상으로 변경됩니다. <br />변경 중 상태에서는 복제 그룹을 작성, 수정, 삭제할 수 없습니다. |
| 이용 중      | 프로필을 이용하여 작성, 변경 중인 복제 그룹이 있는 상태입니다. <br />복제 그룹의 작성, 완료 후 상태는 정상으로 변경됩니다. <br />프로필이 이용중인 상태에서는 프로필을 작성, 수정할 수 없습니다. |

##### 제약 사항
* 설정 정보는 프로필을 생성할 때 **상세 정보 설정** 버튼을 클릭하거나, 프로필을 수정 또는 복사할 때 변경할 수 있습니다.
* 범위를 벗어난 값을 입력하거나 적절하지 않은 항목값을 입력하면 장애가 발생할 수 있습니다. 
* 항목값의 범위나 적절한 입력값은 [Redis 문서](https://redis.io/topics/config) 를 참고하시기 바랍니다.

### 프로필 상세

프로필 상세 정보를 확인할 수 있습니다.

![profile_detail_001.PNG](https://static.toastoven.net/prod_easycache/20.04.28/profile_002.PNG)

* 프로필 상세 정보
    * 항목 이름
    * 기입 예: 항목의 입력 예시
    * 항목 값: 실제 설정된 값
    * 설명: 항목에 대한 설명
* 프로필 항목
    * hash-max-ziplist-entries
    * hash-max-ziplist-value
    * latency-monitor-threshold
    * list-compress-depth
    * list-max-ziplist-size
    * maxmemory-policy
    * maxmemory-samples
    * set-max-intset-entries
    * slowlog-log-slower-than
    * slowlog-max-len
    * tcp-keepalive
    * timeout
    * zset-max-ziplist-entries
    * zset-max-ziplist-value
    * replica-ignore-maxmemory(Redis 5.0 추가)
    * lazyfree-lazy-eviction(Redis 5.0 추가)
    * lazyfree-lazy-expire(Redis 5.0 추가)
    * lazyfree-lazy-server-del(Redis 5.0 추가)
    * repl-backlog-size(Redis 5.0 추가)
    * stream-node-max-bytes(Redis 5.0 추가)
    * stream-node-max-entries(Redis 5.0 추가)
    * client-query-buffer-limit(Redis 5.0 추가)
    * proto-max-bulk-len(Redis 5.0 추가)
    * activedefrag(Redis 5.0 추가)
    * active-defrag-ignore-bytes(Redis 5.0 추가)
    * active-defrag-threshold-lower(Redis 5.0 추가)
    * active-defrag-threshold-upper(Redis 5.0 추가)
    * active-defrag-cycle-min(Redis 5.0 추가)
    * active-defrag-cycle-max(Redis 5.0 추가)
    * active-defrag-max-scan-fields(Redis 5.0 추가)
    * active-expire-effort(Redis 6.0 추가)
    * lazyfree-lazy-user-del(Redis 6.0 추가)

## 알람

EasyCache에서는 원하는 리소스에서 발생하는 특정 이벤트의 알람을 수신 그룹에 전달할 수 있습니다.

![eve_001.PNG](https://static.toastoven.net/prod_easycache/20.04.28/alarm_001.PNG)

### 알람 규칙

알람 발생 조건과 대상, 수신 그룹을 지정합니다.
1. 원하는 알람을 설정하려면 **알람** 탭에서 **알람 규칙 생성** 버튼을 클릭합니다.

2. **알람 규칙 생성** 대화 상자에서 알람 발생 조건과 알람을 수신할 수신 그룹을 지정합니다.
![eve_001.PNG](https://static.toastoven.net/prod_easycache/21.09.16/alarm_002.png)

3. 알람 발생 조건에는 **메트릭 조건**과 **이벤트 조건**, 2가지가 있습니다.

    * **메트릭 조건**: 캐시 인스턴스에서 수집한 각종 성능 지푯값(모니터링 항목 참조)을 이용해 알람 조건을 지정하며 다음과 같은 조건을 지정할 수 있습니다.
        * 메트릭 이름, 연산자, 집계의 종류, 평가의 빈도, 임곗값
    * **이벤트 조건**: 서비스 내에서 발생하는 모든 이벤트 중에서 알람을 받고 싶은 이벤트(이벤트 항목 참조)를 지정할 수 있습니다.

4. **메트릭 조건**과 **이벤트 조건**에 필요한 설정을 결정하기 어렵다면, **알람 템플릿** 에서 제공하는 기본 템플릿을 이용할 수 있습니다.

    * **기본-Standalone**: standalone으로 운영 중인 복제 그룹에 추천하는 알람 발생 조건
    * **기본-HA**: replication으로 운영 중인 복제 그룹에 추천하는 알람 발생 조건

5. **수신 그룹 보기**을 클릭해 수신 그룹을 확인 또는 추가할 수 있습니다.

6. 작성한 알람 규칙은 기본적으로는 모든 복제 그룹이 대상입니다. 특정 복제 그룹용으로 알람 규칙을 작성하려면 **대상 복제 그룹**에 복제 그룹을 지정합니다.

7. 설정 후 **생성** 버튼을 클릭합니다.

생성한 알람 규칙은 알람 기능을 '사용 안 함'으로 변경해 일시적으로 끌 수 있습니다.
알람 규칙은 모든 리전에 동일하게 적용됩니다. 

### 수신 그룹

알람을 받을 수신자를 그룹으로 만들어서 관리할 수 있습니다.

![not_re_001.PNG](https://static.toastoven.net/prod_easycache/20.04.28/alarm_004.PNG)

* 수신 그룹을 확인하려면 **수신 그룹 보기** 버튼을 클릭합니다.
* 원하는 수신 그룹이 없다면 **수신 그룹 생성** 버튼을 클릭해 새로운 수신 그룹을 작성합니다.
* 수신 그룹에서 지정할 수 있는 수신인은 프로젝트 멤버로 한정됩니다.
    * NHN Cloud 회원 정보에 등록한 메일 주소와 전화번호로 메일 또는 SMS를 보낼 수 있습니다.
* 알람 규칙에서 사용 중인 수신 그룹을 삭제하면 다른 수신 그룹이 없는 알람 규칙의 경우 더이상 알람을 보내지 않게 되므로 주의해야 합니다.

생성한 수신 그룹은 모든 리전에서 동일하게 이용할 수 있습니다.

##### 제약 사항

* 알람 규칙의 대상 복제 그룹에 한 개의 복제 그룹만을 입력하고, 해당 복제 그룹을 복제 그룹 화면에서 삭제한 경우, 알람은 유일한 대상 복제 그룹이 없어져 이후부터는 모든 복제 그룹을 대상으로 인식합니다.
* 알람 규칙의 수신 그룹에 한 개의 수신 그룹만 입력하고, 해당 수신 그룹을 수신 그룹 상세 화면에서 삭제한 경우, 알람은 유일한 수신 그룹이 없어져 이후부터는 알람을 보낼 수 없게 됩니다.
* 복제 그룹 생성의 알람은 대상 복제 그룹이 있어도 모든 복제 그룹이 대상이 되어 알람을 보냅니다.
* 프로젝트에 새로운 사용자를 추가할 경우 수신 그룹의 프로젝트 유저 목록에 동기화되기까지 1시간 정도의 대기 시간이 발생할 수 있습니다.
* 알람 규칙의 대상 복제 그룹에 특정 복제 그룹을 지정해도 다른 리전에서는 복제 그룹이 지정되지 않은 상태로 알람 규칙이 동기화돼 모든 복제 그룹에 알람 규칙이 적용됩니다.

## 이벤트

1. **이벤트** 탭을 선택합니다.

2. EasyCache는 복제 그룹에서 발생한 의미 있는 이벤트를 자동으로 남깁니다.

3. 검색어란에 검색할 단어를 입력하고 **검색** 버튼을 클릭하면 이벤트의 리소스 이름과 설명을 대상으로 검색한 결과가 나타납니다.

![eve_002.PNG](https://static.toastoven.net/prod_easycache/20.04.28/event_001.PNG)

* 시간, 날짜별로 검색할 수 있습니다.
* 이벤트 데이터 보존 기간은 1개월입니다.
* 이벤트 타입은 어떤 리소스에서 발생한 이벤트인지를 뜻합니다.
    * ALL: NODE와 REPLICATION_GROUP 관련된 이벤트입니다.
    * NODE: NODE에 관련된 이벤트입니다.
    * REPLICATION_GROUP: REPLICATION_GROUP에 관련된 이벤트입니다.
    * PROFILE: PROFILE에 관련된 이벤트입니다.

##### 이벤트 항목

|타입 | 이벤트   | 이벤트 상세 |
|-----| ------ | ---------------- |
| **복제 그룹** | 삭제   | 시작, 실패, 종료 |
|             | 생성   | 시작, 실패, 종료 |
|             |  수정   | 시작, 실패, 종료 |
|             |  재시작 | 시작, 실패, 종료 |
|             |  그룹 인스턴스 변경 | 시작, 실패, 종료 |
|             |  데이터 가져오기 | 시작, 유저 Object Storage Service 설정 실패, 데이터 파일 다운로드 실패, 지원하지 않는 파일 형식, 가져올 수 없는 RDB 버전, Max memory 부족, 손상된 파일, 노드 재시작 실패, 레플리카 동기화 실패, 종료 |
|             |  데이터 내보내기 | 시작, 유저 Object Storage Service 설정 실패, 데이터 파일 작성 실패, 데이터 파일 업로드 실패, 종료 |
|             |  기존 복제 그룹 복원 | 시작, 데이터 파일 다운로드 실패, 지원하지 않는 파일 형식, 복원할 수 없는 RDB 버전, Max memory 부족, 손상된 파일, 노드 재시작 실패, 레플리카 동기화 실패, 종료 |
|             |  HA 설정 갱신 | 시작, 실패, 종료 |
|             |  버전 업그레이드 | 시작, 실패, 종료 |
|             | 마스터 변경 | 시작, 실패, 종료 |
| **공인 도메인** | 설정 | 시작, 실패, 종료 |
|             | 해제 | 시작, 실패, 종료 |
| **읽기 전용 도메인** | 설정 | 시작, 실패, 종료 |
|             | 해제 | 시작, 실패, 종료 |
| **캐시 인스턴스** | 연결 | 성공, 실패 |
| **노드** | 삭제 | 시작, 실패, 종료 |
|         | 추가 | 시작, 실패, 종료 |
|         | 상태 | 비활성화됨, 활성화됨 |
|         |  노드 인스턴스 변경 | 시작, 실패, 종료 |
| **프로필** | 수정 | 시작, 실패, 종료 |
| **자동 HA** | 삭제 | 시작, 종료 |
|            | 설정 | 시작, 실패, 종료 |
| **장애 조치(failover)** |  | 성공 |
| **백업** | 수동 백업 | 시작, 실패, 종료 |
|        | 자동 백업 | 시작, 실패, 종료 |

## 부록1. 하이퍼바이저 점검을 위한 EasyCache 재시작 가이드

 * EasyCache 노드를 재시작하려면 콘솔에 있는 **재시작** 기능을 사용해야 합니다.
 * 복제 그룹의 재시작 기능으로는 노드가 다른 하이퍼바이저로 이동하지 않습니다. 아래 가이드에 따라 콘솔에 있는 재시작 기능을 이용하시기 바랍니다.

### 1. 점검 대상으로 지정된 노드가 있는 프로젝트로 이동합니다.

### 2. 점검 대상 복제 그룹을 확인 합니다.

* 복제 그룹 이름 옆에 **!재시작** 아이콘이 있는 복제 그룹이 점검 대상 노드가 포함된 복제 그룹 입니다.

![migration_001.png](https://static.toastoven.net/prod_easycache/20.12.01/migration_001.png)

* **!재시작** 아이콘 위에 마우스 커서를 올리시면 자세한 점검 일정을 확인할 수 있습니다.

![migration_002.png](https://static.toastoven.net/prod_easycache/20.12.01/migration_002.png)

### 3. 점검 대상 노드의 타입을 확인합니다.

* 노드 타입 별 재시작에 따른 서비스 영향은 아래와 같습니다.
    * MASTER: **복제 그룹 > 마스터 변경**을 선택해 Master 노드를 Replica 노드로 변경한 뒤 재시작 기능을 이용할 수 있습니다. Standalone의 경우에는 변경하지 않고 이용할 수 있습니다.
    * REPLICA : 서비스에 영향은 없습니다.
    * HA : 재시작되는 동안에 장애 조치(failover) 보장이 되지 않고 장애 조치(failover) 이벤트가 발생할 수 있습니다.
 
* 노드 이동으로 인해 서비스에 영향이 있다고 판단될 경우 NHN Cloud 고객 센터로 연락해 주시면 적합한 조치를 안내해 드리겠습니다.

### 4. 4. 점검 대상 복제 그룹을 선택하고 오른쪽에 표시된 !재시작 아이콘을 클릭하여 점검 노드를 재시작합니다.

![migration_003.png](https://static.toastoven.net/prod_easycache/20.12.01/migration_003.png)

* 점검 노드가 여러 개일 때, 한 번에 노드 1개만 재시작됩니다. HA 노드 먼저 재식작되고, 이후 Replica 노드가 재시작됩니다.

* Standalone의 경우 재시작을 하면 백업 시점의 데이터로 돌아가게 되며, 백업을 하지 않은 경우에는 데이터가 초기화 됩니다.

* Replication의 Master 노드가 점검 대상이면 바로 이용할 수 없습니다. Master 노드를 Replica 노드로 변경한 뒤 재시작합니다.

### 5. 복제 그룹의 상태 표시등이 파란색으로 변하고, !재시작 아이콘이 사라질 때까지 대기합니다.

* 복제 그룹이 재시작되는 동안에는 해당 복제 그룹은 조작을 할 수 없습니다.

## 부록2. 서버 점검을 위한 인터넷게이트웨이 재시작 가이드

* 인터넷게이트웨이를 재시작하려면 콘솔에 있는 **인터넷 게이트웨이 재시작** 기능을 사용해야 합니다.
* 아래 가이드에 따라 콘솔에 있는 재시작 기능을 이용하시기 바랍니다.

### 1. 점검 대상으로 지정된 인터넷 게이트웨이가 있는 프로젝트로 이동합니다.

![gm_001.png](https://static.toastoven.net/prod_easycache/20.12.01/gm_001.png)

### 2. 점검 대상 복제 그룹 탭을 확인 합니다.

* 복제 그룹 탭의 가장 오른쪽에 있는 **!인터넷 게이트웨이 재시작** 아이콘을 확인합니다.
* **!인터넷게이트웨이 재시작** 아이콘 위에 마우스 커서를 올리시면 자세한 점검 일정을 확인할 수 있습니다.

![gm_002.png](https://static.toastoven.net/prod_easycache/20.12.01/gm_002.png)

### 3. 점검 대상 복제 그룹의 오른쪽에 표시된 !인터넷 게이트웨이 재시작 아이콘을 클릭하여 점검 노드를 재시작합니다.

![gm_003.png](https://static.toastoven.net/prod_easycache/20.12.01/gm_003.png)

### 4. !인터넷 게이트웨이 재시작 아이콘이 사라질 때까지 대기합니다.

* 재시작되면 공인 도메인을 사용중인 노드는 외부 네트워크에 접속할 수 없습니다. 공인 도메인을 사용하지 않는 노드에는 영향이 없습니다.
* 재시작에 걸리는 시간은 약 1분입니다. 재시작된 인터넷 게이트웨이는 점검이 완료된 장비에서 구동됩니다.
