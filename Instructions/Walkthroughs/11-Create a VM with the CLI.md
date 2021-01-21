---
wts:
    title: '11 - CLI로 VM 만들기(10분)'
    module: '모듈 03: 핵심 솔루션 및 관리 도구 설명하기'
---
# 11 - CLI로 VM 만들기

이 연습에서는 Azure CLI를 로컬로 설치하고, 리소스 그룹 및 가상 머신을 만들며, Cloud Shell을 사용하고, Azure Advisor 권장 사항을 검토합니다. 

# 태스크 1: Cloud Shell 구성(10분)

이 태스크에서는 Cloud Shell을 구성합니다. 

1. [Azure Portal](https://portal.azure.com)에 로그인합니다.

2. Azure Portal에서 오른쪽 위에 있는 아이콘을 클릭하여 **Azure Cloud Shell** 을 엽니다.

    ![Azure Portal Azure Cloud Shell 아이콘 스크린샷.](../images/1002.png)

3. 이전에 Cloud Shell을 사용한 경우 다음 태스크로 넘어갑니다. 

4. **Bash** 또는 **PowerShell** 을 선택하라는 메시지가 표시되면 **Bash** 를 선택합니다. 

5. 메시지가 표시되면 **스토리지 만들기** 를 클릭하고 Azure Cloud Shell이 초기화될 때까지 기다립니다. 

# 태스크 2: 리소스 그룹 및 가상 머신 만들기

이 태스크에서는 Azure CLI를 사용하여 리소스 그룹과 가상 머신을 만듭니다.  

1. Cloud Shell 창 왼쪽 위 드롭다운 메뉴에서 **Bash** 가 선택되어 있는지 확인합니다(선택되어 있지 않으면 선택).

    ![Bash 드롭다운 메뉴가 강조 표시되어 있는 Azure Portal Azure Cloud Shell 스크린샷.](../images/1002a.png)

2. Bash 세션의 Cloud Shell 창에서 새 리소스 그룹을 만듭니다. 

```cli
az group create --name myRGCLI --location EastUS
```

3. 리소스 그룹이 만들어졌는지 확인합니다.

```cli
az group list --output table
```

4. 새 가상 머신을 만듭니다. 이 명령은 모두 한 줄에 있어야 합니다. 또한 한 줄에 모두 있을 때 틱(`\`) 표시가 없어야 합니다. 

```cli
    az vm create \
    --name myVMCLI \
    --resource-group myRGCLI \
    --image UbuntuLTS \
    --location EastUS \
    --admin-username azureuser \
    --admin-password Pa$$w0rd1234
```

>**참고**: 명령줄을 Windows 컴퓨터에서 사용하는 경우 백슬레시(`\`) 문자를 캐럿(`^`) 문자로 바꿉니다.
    
**참고**: 명령을 완료하는 데 2~3분정도 걸립니다. 명령이 완료되면 가상 머신과 머신에 연결된 다양한 리소스(예: 스토리지, 네트워킹 및 보안 리소스)가 만들어집니다. 가상 머신 배포가 완료될 때까지 다음 단계로 계속 넘어가지 마세요. 

5. 명령의 실행이 완료되면 브라우저 창에서 Cloud Shell 창을 닫습니다.

6. **가상 머신** 을 검색하고 **myVMCLI** 가 실행 중인지 확인합니다.

    ![실행 중인 상태의 myVMPS가 있는 가상 머신 페이지의 스크린샷.](../images/1101.png)


# 작업 3: Cloud Shell에서 명령 실행

이 작업에서는 Cloud Shell에서 CLI 명령을 실행하는 연습을 수행합니다. 

1. Azure Portal 오른쪽 상단의 *Azure Cloud Shell 아이콘* 을 클릭하여 **Azure Cloud Shell** 을 엽니다.

2. 이전에 Cloud Shell을 사용한 경우 5단계로 건너뜁니다. 

3. 이름, 리소스 그룹, 위치 및 상태 등 가상 머신에 대한 정보를 검색합니다. PowerState가 **running** 상태입니다.

```cli
az vm show --resource-group myRGCLI --name myVMCLI --show-details --output table 
```

4. 가상 머신을 중지합니다. 가상 머신이 할당 취소되기 전까지는 청구가 계속된다는 메시지가 표시됩니다. 

```cli
az vm stop --resource-group myRGCLI --name myVMCLI
```

5. 가상 머신 상태를 확인합니다. 이제 PowerState는 **stopped** 여야 합니다.

```cli
az vm show --resource-group myRGCLI --name myVMCLI --show-details --output table 
```

# 작업 4: Azure Advisor 권장 사항 검토

이 작업에서는 Azure Advisor 권장 사항을 검토합니다. 

**참고:** 이전 랩(PowerShell을 사용하여 VM 만들기)을 수행한 경우 이 작업을 이미 완료한 것입니다. 

1. 포털에서 **Advisor** 를 검색하고 선택합니다. 

2. Advisor에서 **개요** 를 선택합니다. 권장 사항이 고가용성, 보안, 성능 및 비용으로 그룹화되어 있습니다. 

    ![Advisor 개요 페이지의 스크린샷. ](../images/1103.png)

3. **모든 권장 사항** 을 선택하고 각 권장 사항 및 제안되는 작업을 검토합니다. 

    **참고:** 권장 사항은 리소스에 따라 다를 수 있습니다. 

    ![Advisor 모든 권장 사항 페이지의 스크린샷. ](../images/1104.png)

4. 권장 사항을 CSV 또는 PDF 파일로 다운로드할 수 있습니다. 

5. 경고를 만들 수도 있습니다. 

6. 시간이 있으면 Azure CLI를 사용하여 실험을 계속하십시오.

축하합니다! 로컬 머신에 PowerShell을 설치하고, PowerShell을 사용하여 가상 머신을 만들었으며, PowerShell 명령을 실행하고, Advisor 권장 사항을 검토했습니다.

**참고**: 추가 비용을 방지하려면 이 리소스 그룹을 제거할 수 있습니다. 리소스 그룹을 검색하고 리소스 그룹을 클릭한 다음 **리소스 그룹 삭제** 를 클릭합니다. 리소스 그룹의 이름을 확인한 다음 **삭제** 를 클릭합니다. **알림** 을 모니터링하여 삭제가 어떻게 진행되는지 확인합니다.
