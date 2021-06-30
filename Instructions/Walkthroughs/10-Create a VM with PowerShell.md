---
wts:
    title: '10 - PowerShell로 VM 만들기(10분)'
    module: '모듈 03: 핵심 솔루션 및 관리 도구 설명'
---
# 10 - PowerShell로 VM 만들기

이 연습에서는 Cloud Shell을 구성하고, Azure PowerShell 모듈을 사용하여 리소스 그룹 및 가상 머신을 만들고, Azure Advisor 권장 사항을 검토합니다. 

# 작업 1: Cloud Shell 구성(10분)

이 작업에서는 Cloud Shell을 구성합니다. 

1. [Azure Portal](https://portal.azure.com)에 로그인합니다.

2. Azure Portal에서 오른쪽 상단의 아이콘을 클릭하여 **Azure Cloud Shell**을 엽니다.

    ![Azure Portal의 Azure Cloud Shell 아이콘 스크린샷.](../images/1002.png)

3. 이전에 Cloud Shell을 사용한 경우 다음 작업으로 넘어갑니다. 

4. **Bash** 또는 **PowerShell** 을 선택하라는 메시지가 표시되면 **PowerShell** 을 선택합니다.

5. 메시지가 표시되면 **스토리지 만들기**를 클릭하고 Azure Cloud Shell이 초기화될 때까지 기다립니다. 

# 작업 2: 리소스 그룹 및 가상 머신 만들기

이 작업에서는 PowerShell을 사용하여 리소스 그룹과 가상 머신을 만듭니다.  

1. Cloud Shell 창의 왼쪽 위 드롭다운 메뉴에서 **PowerShell**이 선택되어 있는지 확인합니다.

2. Cloud Shell 창 내의 PowerShell 세션에서 새 리소스 그룹을 만듭니다. 

    ```PowerShell
    New-AzResourceGroup -Name myRGPS -Location EastUS
    ```

3. 새 리소스 그룹을 확인합니다. 

    ```PowerShell
    Get-AzResourceGroup | Format-Table
    ```

4. 가상 머신을 만듭니다. 메시지가 표시되면 해당 가상 머신에서 로컬 관리자 계정으로 구성될 사용자 이름(**azureuser**) 및 암호(**Pa$$w0rd1234**)를 입력합니다. 마지막 줄을 제외한 각 줄의 끝에 틱(`) 문자를 포함해야 합니다(전체 명령을 한 줄에 입력하는 경우에는 틱 문자가 있어서는 안 됨).

    ```PowerShell
    New-AzVm `
    -ResourceGroupName "myRGPS" `
    -Name "myVMPS" `
    -Location "East US" `
    -VirtualNetworkName "myVnetPS" `
    -SubnetName "mySubnetPS" `
    -SecurityGroupName "myNSGPS" `
    -PublicIpAddressName "myPublicIpPS"
    ```
** VM이 배포될 때까지 기다린 후에 PowerShell을 닫습니다.

5. PowerShell 세션 Cloud Shell 창을 닫습니다.

6. Azure Portal에서 **가상 머신**을 검색하고 **myVMPS**가 실행 중인지 확인합니다. 몇 분 정도 걸릴 수 있습니다.

    ![myVMPS가 실행 중인 가상 머신 페이지의 스크린샷.](../images/1001.png)

7. 새 가상 머신에 액세스하고 개요 및 네트워킹 설정을 검토하여 정보가 올바르게 배포되었는지 확인합니다. 

# 작업 3: Cloud Shell에서 명령 실행

이 작업에서는 Cloud Shell에서 PowerShell 명령을 실행하는 연습을 합니다. 

1. Azure Portal에서 오른쪽 상단의 아이콘을 클릭하여 **Azure Cloud Shell**을 엽니다.

2. Cloud Shell 창의 왼쪽 위 드롭다운 메뉴에서 **PowerShell**이 선택되어 있는지 확인합니다.

3. 이름, 리소스 그룹, 위치 및 상태 등 가상 머신에 대한 정보를 검색합니다. PowerState가 **running** 입니다.

    ```PowerShell
    Get-AzVM -name myVMPS -status | Format-Table -autosize
    ```

4. 가상 머신을 중지합니다. 메시지가 표시되면 작업을 확인(Yes)합니다. 

    ```PowerShell
    Stop-AzVM -ResourceGroupName myRGPS -Name myVMPS
    ```

5. 가상 머신 상태를 확인합니다. 이제 PowerState가 **deallocated**여야 합니다. 포털에서 가상 머신 상태를 확인할 수도 있습니다. 

    ```PowerShell
    Get-AzVM -name myVMPS -status | Format-Table -autosize
    ```

# 작업 4: Azure Advisor 권장 사항 검토

**참고:** 이 작업은 Azure CLI 랩을 사용하여 VM 만들기 작업과 동일합니다. 

이 작업에서는 가상 머신에 대한 Azure Advisor 권장 사항을 검토합니다. 

1. **모든 서비스** 블레이드에서 **Advisor**를 검색하고 선택합니다. 

2. **Advisor** 블레이드에서 **개요**를 선택합니다. 권장 사항이 고가용성, 보안, 성능 및 비용으로 그룹화되어 있습니다. 

    ![Advisor 개요 페이지의 스크린샷. ](../images/1003.png)

3. **모든 권장 사항**을 선택하고 각 권장 사항 및 제안되는 작업을 검토합니다. 

    **참고:** 권장 사항은 리소스에 따라 다를 수 있습니다. 

    ![Advisor 모든 권장 사항 페이지의 스크린샷. ](../images/1004.png)

4. 권장 사항을 CSV 또는 PDF 파일로 다운로드할 수 있습니다. 

5. 경고를 만들 수도 있습니다. 

6. 시간이 있으면 Azure PowerShell을 사용하여 실험을 계속하세요. 

축하합니다. Cloud Shell을 구성하고, PowerShell을 사용하여 가상 머신을 만들고, PowerShell 명령을 실행하고, Advisor 권장 사항을 검토했습니다.

**참고**: 추가 비용을 방지하려면 이 리소스 그룹을 제거할 수 있습니다. 리소스 그룹을 검색하고 리소스 그룹을 클릭한 다음 **리소스 그룹 삭제**를 클릭합니다. 리소스 그룹의 이름을 확인한 다음 **삭제**를 클릭합니다. **알림**을 모니터링하여 삭제가 어떻게 진행되는지 확인합니다.
