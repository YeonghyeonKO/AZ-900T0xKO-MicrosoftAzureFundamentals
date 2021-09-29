---
wts:
    title: '11 - CLI로 VM 만들기(10분)'
    module: '모듈 03: 핵심 솔루션 및 관리 도구 설명'
---
# 11 - CLI로 VM 만들기(10분)

이 연습에서는 Cloud Shell을 구성하고, Azure CLI 모듈을 사용하여 리소스 그룹 및 가상 머신을 만들고, Azure Advisor 권장 사항을 검토합니다. 

# 작업 1: Cloud Shell 구성 

이 작업에서는 Cloud Shell을 구성한 다음 Azure CLI를 사용하여 리소스 그룹과 가상 머신을 만듭니다.  

1. [Azure Portal](https://portal.azure.com)에 로그인합니다.

2. Azure Portal에서 오른쪽 상단의 아이콘을 클릭하여 **Azure Cloud Shell**을 엽니다.

    ![Azure Portal의 Azure Cloud Shell 아이콘 스크린샷.](../images/1002.png)
   
3. Azure Cloud Shell 시작하기 대화 상자에서 메시지가 표시되면 **Bash** 또는 **PowerShell**을 선택하고 **Bash**를 선택합니다. 

4. 새 창이 열리면서 **탑재된 스토리지 없음**이라는 메시지가 표시됩니다. **고급 설정**을 선택합니다.

5. 고급 설정 화면에서 다음 필드를 채우고 스토리지 만들기를 클릭합니다.
    - 리소스 그룹: **새 리소스 그룹 만들기**
    - 스토리지 계정: 전역적으로 고유한 이름을 사용하는 새 계정 만들기(예: loudshellxyzstorage)
    - 파일 공유: 새 파일을 만들고 이름을 cloudshellfileshare로 지정합니다.


# 작업 2: CLI를 사용하여 가상 머신을 만들기

이 작업에서는 Azure CLI를 사용하여 리소스 그룹과 가상 머신을 만듭니다.

1. Cloud Shell 창의 왼쪽 위 드롭다운 메뉴에서 **Bash** 가 선택되어 있는지 확인합니다(그렇지 않다면 선택함).

    ![Bash 드롭다운이 강조 표시된 Azure Portal Azure Cloud Shell의 스크린샷.](../images/1002a.png)


2. 다음 명령을 입력하여 사용 중인 리소스 그룹을 확인합니다.

    ```cli
    az group list --output table
    ```

4. Cloud Shell에 아래 명령을 입력하고 각 줄(마지막 줄 제외) 다음에 백슬래시(`\`) 문자가 있는지 확인합니다. 전체 명령을 한 줄에 입력하는 경우에는 백슬래시 문자를 사용하지 마세요. 

    ```cli
    az vm create \
    --name myVMCLI \
    --resource-group myRGCLI \
    --image UbuntuLTS \
    --location EastUS2 \
    --admin-username azureuser \
    --admin-password Pa$$w0rd1234
    ```

    >**참고**: Windows 컴퓨터에서 명령줄을 사용하는 경우에는 백슬래시(`\`) 문자를 캐럿(`^`) 문자로 바꾸세요.

    **참고**: 명령을 완료하는 데 2~3분 정도 걸립니다. 명령이 완료되면 가상 머신과 머신에 연결된 다양한 리소스(예: 스토리지, 네트워킹 및 보안 리소스)가 만들어집니다. 가상 머신 배포가 완료될 때까지 다음 단계로 진행하지 마십시오. 

5. 명령 실행이 완료되면 브라우저 창에서 Cloud Shell 창을 닫습니다.

6. Azure Portal에서 **가상 머신**을 검색하고 **myVMCLI**가 실행 중인지 확인합니다.

    ![myVMPS가 실행 중인 가상 머신 페이지의 스크린샷.](../images/1101.png)


# 작업 3: Cloud Shell에서 명령 실행

이 작업에서는 Cloud Shell에서 CLI 명령을 실행하는 연습을 합니다. 

1. Azure Portal에서 오른쪽 상단의 아이콘을 클릭하여 **Azure Cloud Shell** 을 엽니다.

2. Cloud Shell 창의 왼쪽 위 드롭다운 메뉴에서 **Bash** 가 선택되어 있는지 확인합니다.

3. 이름, 리소스 그룹, 위치 및 상태 등 프로비저닝한 가상 머신에 대한 정보를 검색합니다. PowerState가 **running** 입니다.

    ```cli
    az vm show --resource-group myRGCLI --name myVMCLI --show-details --output table 
    ```

4. 가상 머신을 중지합니다. 가상 머신이 할당 취소되기 전까지는 청구가 계속된다는 메시지가 표시됩니다. 

    ```cli
    az vm stop --resource-group myRGCLI --name myVMCLI
    ```

5. 가상 머신 상태를 확인합니다. 이제 PowerState가 **stopped** 여야 합니다.

    ```cli
    az vm show --resource-group myRGCLI --name myVMCLI --show-details --output table 
    ```

# 작업 4: Azure Advisor 권장 사항 검토

이 작업에서는 Azure Advisor 권장 사항을 검토합니다.

   **참고:** 이전 랩(PowerShell로 VM 만들기)를 완료했다면 이미 이 작업을 수행한 것입니다. 

1. **모든 서비스** 블레이드에서 **Advisor** 를 검색하고 선택합니다. 

2. **Advisor** 블레이드에서 **개요** 를 선택합니다. 권장 사항이 고가용성, 보안, 성능 및 비용으로 그룹화되어 있습니다. 

    ![Advisor 개요 페이지의 스크린샷. ](../images/1103.png)

3. **모든 권장 사항** 을 선택하고 각 권장 사항 및 제안되는 작업을 검토합니다. 

    **참고:** 권장 사항은 리소스에 따라 다를 수 있습니다. 

    ![Advisor 모든 권장 사항 페이지의 스크린샷. ](../images/1104.png)

4. 권장 사항을 CSV 또는 PDF 파일로 다운로드할 수 있습니다. 

5. 경고를 만들 수도 있습니다. 

6. 시간이 있으면 Azure CLI를 사용하여 실험을 계속하세요. 

축하합니다. Cloud Shell을 구성하고, Azure CLI를 사용하여 가상 머신을 만들고, Azure CLI 명령을 실행하고, Advisor 권장 사항을 검토했습니다.

**참고**: 이 리소스 그룹을 제거해 추가 비용이 발생하는 것을 방지할 수도 있습니다. 리소스 그룹을 검색하고 리소스 그룹을 클릭한 다음 **리소스 그룹 삭제**를 클릭합니다. 리소스 그룹의 이름을 확인한 다음 **삭제**를 클릭합니다. **알림**을 모니터링하여 삭제가 어떻게 진행되는지 확인합니다.
