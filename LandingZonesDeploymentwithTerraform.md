

> [实验背景](#实验背景)
>
> [实验环境准备](#实验环境准备)
>
> [动手实验](#动手实验)
>
> [考虑](#考虑)

## 实验背景

本实验目的意在帮助云管理Partner及客户使用Azure
Terraform（包含工具集及代码）部署一个企业级Landing
Zone，以此掌握使用云原生IaC（基础架构即代码）的技能加速企业客户云采用历程。

在本动手实验中，您将了解如何使用Azure
Terraform为虚拟企业客**Contoso**公司构建基于云采用框架
(CAF)的登陆区。需要同时具备**Terraform 、Azure
CAF、Docker、Git知识，以及具有简单的开发技能**。

本动手实验将指导您如何部署平台Landing Zone，包括：

-   Azure 远程状态管理（启动板）

-   用于管理组、策略定义、策略分配和 RBAC 的 Azure Landing
    Zone（以前称为企业级）

-   身份服务（Azure Active Directory 服务，扩展您的本地 Active Directory
    域服务\...\...）

-   监控服务

-   连接服务（虚拟WAN、私有DNS区域、出口防火墙\...）

本动手实验中的步骤将指导您完成以下过程：

![Azure
登陆区域环境的创建](./media/media/image1.png)

## 所需基础知识介绍

1.  **Azure CAF Landing Zone（登陆区）**

> Microsoft Azure
> CAF（云采用框架）为您提供采用Azure的指导和最佳实践。而AzureLanding
> Zone是多订阅Azure环境的输出，该环境负责缩放、安全治理、网络和标识。AzureLanding
> Zone可在Azure中实现应用程序迁移、现代化和创新。此方法考虑支持客户应用程序组合所需的所有平台资源，并且不会区分基础结构即服务或平台即服务。

![图形用户界面
低可信度描述已自动生成](./media/media/image2.png)

![图形用户界面, 图示
描述已自动生成](./media/media/image3.png)

2.  **Azure Terraform**

Azure提供用于生成Azure登陆区的微软第一方服务，也可以使用第三方工具完成这项工作。
HashiCorp的**Terraform**是客户和合作伙伴经常用来部署Landing
Zone的一种工具。

Azure Landing Zone (ALZ) Terraform 模块是用于从 Azure Landing
Zone概念体系结构部署平台资源的正式 Terraform 模块。
该模块旨在简化管理组层次结构、策略和管理订阅中的资源的部署。
将资源部署到应用程序Landing
Zone超出了模块的范围，从而决定部署方法和工具给负责应用程序的团队。

![图形用户界面, 应用程序
描述已自动生成](./media/media/image4.png)

CAF Terraform Landing Zone方法是一组工具，提供抽象的、有意见的 Terraform
实现，以在 Azure 中提供部署自动化。 它允许客户使用 Terraform
将资源部署到应用程序Landing Zone，并提供部署订阅的机制。
此方法还可以部署 Azure Landing Zone概念体系结构，方法是实现 Azure
Landing Zone Terraform 模块。

![图形用户界面, 应用程序
描述已自动生成](./media/media/image5.png)

下图演示了两种方法的覆盖范围：

![Terraform module
comparison](./media/media/image6.png)

> CAF Terraform 登陆区产品团队的使命是：

-   为 Azure 上的 Terraform 配备站点可靠性工程团队。

-   使 IaC 民主化：基础设施即配置。

-   将状态管理和企业范围内的组合产品化。

-   使用 Azure 企业级Landing Zone标准化部署。

-   使用本地 Terraform 和 DevOps 实施Azure企业级登陆区设计方法。

-   就如何在Microsoft Azure上为IaC（基础设施即代码）启用 DevOps
    提出规范性指南。

-   使用一组通用实践和共享最佳实践来培养 Azure
    Terraform社区成员和开发者。

3.  **CAF Rover**

在您的笔记本电脑上使用原生的Azure Terraform
可以完成你想要的Azure资源管理。但为了简化使用Terraform创建Landing Zone，
**CAF** **Rover提供了对Azure Terraform近一步封装，**并且具有两个维度：

-   **Rover运行环境是一个容器**

    -   允许在 PC、Mac、Linux
        上获得一致的开发人员体验，包括正确的工具、git 挂钩和 DevOps
        工具。

    -   与Visual Studio Code、GitHub Codespaces的本机集成。

    -   包含应用Landing Zone所需的版本化工具集。

    -   通过分离运行环境和配置环境，帮助您快速切换组件版本。

    -   确保管道的普遍性和抽象性在任何地方运行Rover，无论使用哪种管道技术。

-   **Rover是一个 Terraform 包装器**

    -   帮助您在 Azure 存储帐户上透明地存储和检索 Terraform 状态文件。

    -   促进向 CI/CD 的过渡。

    -   在本地和管道内部实现无缝体验（状态连接、执行跟踪等）。

**CAF Rover有如下好处**：

-   极大地简化了 Azure 存储帐户的安全状态管理。

-   帮助测试不同版本的二进制文件（新版本的 Terraform、Azure
    CLI、jq、tflint 等）

-   无处不在的开发环境：每个人都使用相同版本的 DevOps
    工具链，始终保持最新，在笔记本电脑、管道、GitHub 代码空间等上运行。

-   促进向任何 CI/CD 的身份转换：即所有 CI/CD 都具有容器功能。

-   允许从一个 DevOps 环境轻松过渡到另一个（GitHub Actions、Azure
    DevOps、Jenkins、CircleCI 等）

-   它是开源的，并利用了 Terraform 经常需要的开源项目。在哪里可以找到
    CAF
    Rover[？](https://aztfmod.github.io/documentation/docs/rover/rover-intro#where-to-find-caf-rover)

Rover一个开源项目，您可以直接从Docker
Hub找到稳定或预览版本，或者创建您自己的版本，以匹配您组织自己的 DevOps
工具包。您可以在此处找到Rover项目。

您可以单独使用此处的所有工具，但这意味着您必须手动完成Rover所做的一切:)

Rover 已经包含在 CAF Terraform
的开发环境中（.devcontainer各个项目中的文件夹）。

4.  **Rover Ignite**

为客户云环境创建一致的配置文件堆栈很容易出错。Rover ignite
是一个迭代工具，为您创建部署完特定整客户环境所需的文件集。Rover ignite
命令将采用模板化配置文件，并根据您的客户环境设置为您生成不同的元素，包括readme文件。

Rover ignite 将 YAML 文件作为模板摄取，这些模板将生成 tfvars
文件、readme文件和即将生成的管道。

![火星车点火概述](./media/media/image7.png)

总结下来，Rover
Ignite生成基于客户环境所需的Rover变量及含有运行Rover命令的readme文件等客户化配置信息。Readme文档里含有执行命令及描述。基于这些执行命令，可以通过复制/粘贴快速执行Rover命令进行部署。

典型的Rover Ignite命令如下所示（但是，一般来说，在 CAF
中，我们将为您提供有关如何最好地使用它的具体说明）：

rover ignite \\\
\--playbook /tf/caf/landingzones/templates/platform/ansible.yaml \\\
-e *base_templates_folder*=/tf/caf/landingzones/templates/platform \\\
-e *resource_template_folder*=/tf/caf/landingzones/templates/resources
\\\
-e *config_folder*=/tf/caf/definitions/single_reuse/platform \\\
-e *landingzones_folder*=/tf/caf/landingzones

所需参数参考：

  --------------------------------------------------------------------------------------
  **参数**                   **是否必需**   **描述**
  -------------------------- -------------- --------------------------------------------
  -playbook                  是             根配置 Ansible playbook 的路径。

  -e base_templates_folder   是             平台 Terraform Landing Zone的 Ansible
                                            模板集的路径。

  -e                         是             用于 Azure 资源实例化的 Jinja 模板集的路径。
  resource_template_folder                  

  -e config_folder           是             登陆区的功能模板集的路径 -
                                            取决于您从登陆区内的模板目录中选择的方案。

  -e landingzones_folder     是             登陆区逻辑文件夹的根路径。
  --------------------------------------------------------------------------------------

## 实验环境准备

1.  实验所需Azure权限

> Azure AD：全局管理员
>
> Azure 订阅：1 个具有所有者权限的订阅。
>
> 管理组：分支或根管理组的"管理组贡献者"权限。

2.  Rover运行环境需要Docker，所以本实验需要在您的本地电脑环境下载并运行[Docker](https://docs.docker.com/get-docker/)

3.  如果还没有[Git](https://git-scm.com/downloads)和[VS
    Code](https://code.visualstudio.com/download)，请先点击各自链接下载。

4.  Fork包含实验代码（IaC）的Repo

Rover
Starter项目是一个空环境，可让您开始使用初始配置文件并创建连贯的堆栈。为方便中国合作伙伴访问Rover
Starter代码仓库，可以任意选择在[GitHub](https://github.com/Azure/caf-terraform-landingzones-platform-starter)或[Gitee](https://gitee.com/mirrors_Azure/caf-terraform-landingzones-platform-starter)
**Fork**到你的Github或Gitee账户下，并设置成私有仓库，用于实验中模拟的IaC代码仓库

[Github上 Cloud Adoption Framework landing zones for Terraform -
Platform starter
template](https://github.com/Azure/caf-terraform-landingzones-platform-starter)如下：

![图形用户界面, 文本, 网站
描述已自动生成](./media/media/image8.png)

[Gitee 上Cloud Adoption Framework landing zones for Terraform - Platform
starter
template](https://gitee.com/mirrors_Azure/caf-terraform-landingzones-platform-starter)如下：

![电脑萤幕的截图
描述已自动生成](./media/media/image9.png)

5.  在以下命令行\<org>/\<repo> 位置替换成你的github或gitee仓库地址

git clone git://github.com/\<org>/\<repo> contoso && cd contoso

你应该观察到：

Cloning into \'contoso\'\...\
remote: Enumerating objects: 429, done.\
remote: Counting objects: 100% (429/429), done.\
remote: Compressing objects: 100% (320/320), done.\
remote: Total 429 (delta 110), reused 307 (delta 77), pack-reused 0\
Receiving objects: 100% (429/429), 2.93 MiB \| 1.52 MiB/s, done.\
Resolving deltas: 100% (110/110), done.

6.  从 contoso 文件夹打开 Visual Studio Code

code .

根据下图提示选择**信任存储库**

![文本
描述已自动生成](./media/media/image10.png)

Visual Studio Code

7.  在Visual Studio code打开克隆的存储库并显示以下结构。

![电脑屏幕的截图
描述已自动生成](./media/media/image11.png)

8.  添加VS Code远程开发extension插件。选择**Remote --
    Containers**插件并单击安装。

![电脑屏幕的手机截图
描述已自动生成](./media/media/image12.png)

9.  在VS code顶部，单击查看-\>命令面板

![电脑屏幕的截图
描述已自动生成](./media/media/image13.png)

10. 输入Remote-Containers，点击它。Rover容器将被启动。

![](./media/media/image14.png)

11. 您现在应该看到以下终端（Terminal）。您将在此终端运行本实验中描述的所有终端命令。

![图片包含 图形用户界面
描述已自动生成](./media/media/image15.png)

12. 克隆 CAF Terraform
    landingzones[代码](https://aztfmod.github.io/documentation/docs/azure-landing-zones/landingzones/platform/org-setup#clone-the-caf-terraform-landingzones-code)。现在您已经准备好使用配置文件夹，让我们克隆我们将用于运行命令的登陆区的逻辑（Terraform
    代码）。

**注意：**CAF Terraform Landing Zone框架假定Landing Zone Terraform
代码被克隆到名为**landingzones**
的存储库中。不要使用其他名称作**landingzones**，这是用于驱动一致性的约定。

Github:

git clone https://github.com/Azure/caf-terraform-landingzones.git
landingzones

Gitee:

git clone https://gitee.com/teo-ma/caf-terraform-landingzones.git
landingzones

看到代码正在被clone到**landingzones目录**

Cloning into \'landingzones\'\...\
remote: Enumerating objects: 9067, done.\
remote: Counting objects: 100% (393/393), done.\
remote: Compressing objects: 100% (281/281), done.\
remote: Total 9067 (delta 161), reused 295 (delta 108), pack-reused
8674\
Receiving objects: 100% (9067/9067), 11.65 MiB \| 6.83 MiB/s, done.\
Resolving deltas: 100% (5792/5792), done.\
Updating files: 100% (406/406), done.

13. 进入landingzones目录，请注意，所有文件夹都以 devcontainers 中的
    /tf/caf 开头。

\# caf git:(main) ✗ cd landingzones\
➜ landingzones git:(main) ✗ pwd\
/tf/caf/landingzones\
➜ landingzones git:(main) ✗

14. CAF Terraform 登陆区会定期发布。为了对齐部署说明，您需要确保
    Terraform
    代码也使用正确的分支或标记。在终端中，运行以下命令来选择分支,以便切换到选定的landingzones版本

git checkout 2203.0\
\
Note: switching to \'2203.0\'.\
\
You are *in* \'detached HEAD\' state. You can look around, make
experimental\
changes and commit them, and you can discard any commits you make *in*
this\
state without impacting any branches by switching back to a branch.

## 动手实验

1.  本实验的目的是让您开始使用单订阅环境，该环境将部署全部功能，并允许您试验Landing
    Zone机制和跨状态组合。它将创建一个平台定义，然后您可以根据需要进行自定义，向您显示具有生产和非生产环境的单个
    Azure 区域。

2.  第一步是在命令行登录到你的 Azure 环境，你可以简单地运行

**注意**：本动手实验文档默认按Azure Global环境运行，Rover支持Azure
Global和Azure
China（世纪互联）等所有Azure公有云，在rover命令行登陆azure之前需要设置所使用的云环境

**Azure Global ：**

az cloud set -n AzureCloud

**Azure China ：**

az cloud set -n AzureChinaCloud

通过命令行运行rover login登录Azure

rover login

单击
URL [[https://microsoft.com/devicelogin]{.underline}](https://microsoft.com/devicelogin)，设置代码并使用您的
Azure 帐户进行身份验证。

![文本
描述已自动生成](./media/media/image16.png)

3.  登录成功后，您会看到Rover显示您的 Azure
    环境的上下文。验证一切是否正确。您可以查看Rover输出，确认 AAD
    和订阅的经过身份验证的上下文，以及可能的下一个命令。

![文本
描述已自动生成](./media/media/image17.png)

4.  现在让我们从landingzones中选择正确的配置文件示例并将其放入我们的配置存储库中。

只需运行以下命令：

\'/tf/caf/landingzones/templates/platform/deploy_platform.sh\'

5.  第一次运行该命令时，系统会提示您几个简单的问题，如下所示：

\[WARNING\]: No inventory was parsed, only implicit localhost is
available\
\[WARNING\]: provided hosts list is empty, only localhost is available.
Note that the implicit localhost does not match \'all\'\
Set the short version of your customer name with no spaces \[contoso\]:\
Set the CAF Environment value \[contoso\]:\
Set the prefix to add to all resource. \[caf\]:\
Management group prefix (value must be between 2 to 10 characters long
and can only contain alphanumeric characters and hyphens). \[es\]:\
Management group name \[Contoso\]:\
Email address to send all notifications \[email\@address.com\]:\
Azure regions (lowercase, short version) \[{\'region1\':
\'southeastasia\', \'region2\': \'eastasia\'}\]:\
Default CAF Azure region key \[region1\]:

6.  完成后，您可以转到launchpad readme文件。

![启动板入门](./media/media/image18.png)

7.  查看和客户化定义文件

/tf/caf/platform/definition/GETTING-STARTED.md

现在生成定义文件。它包含一组 YAML 文件，可以让您轻松入门。

8.  触发rover ignite

完成此步骤后，您必须遵循存储库
( **/tf/caf/platform/definition/GETTING-STARTED.md** ) 中的 readme.md
并按照说明进行操作。使用 rover ignite 生成 Terraform
配置文件和自定义自述文件的第一步：

ansible-playbook /tf/caf/landingzones/templates/ansible/ansible.yaml \\\
\--extra-vars \"@/tf/caf/platform/definition/ignite.yaml\"

rover ignite 的输出将开始创建目标配置文件夹结构和 Terraform
文件，如下所示：

TASK \[\[level0-launchpad\] Clean-up directory\]
\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\
skipping: \[localhost\]\
\
TASK \[\[level0-launchpad\] Creates directory\]
\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\
changed: \[localhost\]\
\
TASK \[\[level0-launchpad\] - resources - resource_groups\]
\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\
changed: \[localhost\] =>
(item=/tf/caf/landingzones/templates/resources/resource_groups.tfvars.j2)\
\
TASK \[\[level0-launchpad\] launchpad\]
\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\
changed: \[localhost\] => (item=dynamic_secrets)\
changed: \[localhost\] => (item=global_settings)\
changed: \[localhost\] => (item=keyvaults)\
changed: \[localhost\] => (item=landingzone)\
changed: \[localhost\] => (item=role_mappings)\
changed: \[localhost\] => (item=storage_accounts)\
\
TASK \[\[level0-launchpad\] Clean-up identity files\]
\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\
skipping: \[localhost\] => (item=azuread_api_permissions)\
skipping: \[localhost\] => (item=azuread_applications)\
skipping: \[localhost\] => (item=azuread_group_members)\
skipping: \[localhost\] => (item=azuread_groups)\
skipping: \[localhost\] => (item=azuread_roles)\
skipping: \[localhost\] => (item=keyvault_policies)\
skipping: \[localhost\] => (item=service_principals)\
\
TASK \[\[level0-launchpad\] lauchpad - identity - service_principal\]
\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\
changed: \[localhost\] => (item=azuread_api_permissions)\
changed: \[localhost\] => (item=azuread_applications)\
changed: \[localhost\] => (item=azuread_group_members)\
changed: \[localhost\] => (item=azuread_groups)\
changed: \[localhost\] => (item=azuread_roles)\
changed: \[localhost\] => (item=keyvault_policies)\
changed: \[localhost\] => (item=service_principals)\
\
TASK \[\[level0-launchpad\] Deploy the launchpad\]
\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\
skipping: \[localhost\]\
\
TASK \[\[level0-launchpad\] Get tfstate account name\]
\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\
changed: \[localhost\]

**注意：**在第一次执行 rover ignite
命令时，您会注意到一些红色错误。这是可以预料的，因为rover
ignite正在尝试寻找launchpad和已部署的服务。但目前尚未部署任何东西。

TASK \[\[level0-launchpad\] Get launchpad tfstate details\]
\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\
fatal: \[localhost\]: FAILED! => {\"changed\": true, \"cmd\": \"az
storage blob download \--name \\\"caf_launchpad.tfstate\\\"
\--account-name \\\"\\\" \--container-name \\\"tfstate\\\" \--auth-mode
\\\"login\\\" \--file
\\\"\~/.terraform.cache/launchpad/caf_launchpad.tfstate\\\"\\n\",
\"delta\": \"0:00:01.796026\", \"end\": \"2022-01-20 10:12:52.623103\",
\"msg\": \"non-zero return code\", \"rc\": 1, \"start\": \"2022-01-20
10:12:50.827077\", \"stderr\": \"ERROR: \\nMissing credentials to access
storage service. The following variations are accepted:\\n (1) account
name and key (\--account-name and \--account-key options or\\n set
AZURE_STORAGE_ACCOUNT and AZURE_STORAGE_KEY environment variables)\\n
(2) account name and SAS token (\--sas-token option used with either the
\--account-name\\n option or AZURE_STORAGE_ACCOUNT environment
variable)\\n (3) account name (\--account-name option or
AZURE_STORAGE_ACCOUNT environment variable;\\n this will make calls to
query for a storage account key using login credentials)\\n (4)
connection string (\--connection-string option or\\n set
AZURE_STORAGE_CONNECTION_STRING environment variable); some shells will
require\\n quoting to preserve literal character interpretation.\",
\"stderr_lines\": \[\"ERROR: \", \"Missing credentials to access storage
service. The following variations are accepted:\", \" (1) account name
and key (\--account-name and \--account-key options or\", \" set
AZURE_STORAGE_ACCOUNT and AZURE_STORAGE_KEY environment variables)\", \"
(2) account name and SAS token (\--sas-token option used with either the
\--account-name\", \" option or AZURE_STORAGE_ACCOUNT environment
variable)\", \" (3) account name (\--account-name option or
AZURE_STORAGE_ACCOUNT environment variable;\", \" this will make calls
to query for a storage account key using login credentials)\", \" (4)
connection string (\--connection-string option or\", \" set
AZURE_STORAGE_CONNECTION_STRING environment variable); some shells will
require\", \" quoting to preserve literal character interpretation.\"\],
\"stdout\": \"\", \"stdout_lines\": \[\]}\
\...ignoring\
\
TASK \[\[level0-launchpad\] Get subscription_creation_landingzones
details\]
\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\
skipping: \[localhost\]

9.  部署launchpad（level0 ）。转到
    /tf/caf/configuration/contoso/platform/level0/launchpad/readme.md

![文本
描述已自动生成](./media/media/image19.png)

在readme中您会看到基于你的Azure环境生成了客户化的landing
zone部署命令和解释说明，根据readme描述提示，一步接一步的运行命令进行部署。

![图形用户界面, 文本
描述已自动生成](./media/media/image20.png)

**注意：**每次通过rover命令执行正式部署（-a apply）前,要先通过rover -a
plan命令进行环境检查和部署变化，以便确保部署成功和预先了解部署前后状态变化。观测下面两个Rover命令的参数变化：

![文本
描述已自动生成](./media/media/image21.png)

10. 运行 rover xxxxxxx -a
    plan命令，第一次执行过程可能需要10分钟甚至更长时间，期间会检查Azure账户信息，环境状态信息和部署前后变化等，以及所需依赖包及版本，需要的话可能会下载相应的依赖包。

![文本
描述已自动生成](./media/media/image22.png)

11. Plan命令成功结束后，观察输出的文本信息，可以看到部署前后将会产生的变化信息：

![文本
描述已自动生成](./media/media/image23.png)

12. 接下来正式通过apply 执行部署landing zone launchpad

![图形用户界面, 文本
描述已自动生成](./media/media/image24.png)

13. apply
    命令成功结束后，观察输出的文本信息，可以看到部署前后将会产生的变化信息：

![文本
描述已自动生成](./media/media/image25.png)

14. 登录Azure
    Portal，观测生成的资源组和资源信息，生成了3个资源组，每个资源组含有一个Key
    vault和Storage
    account，用于保存身份信息和存储部署状态信息，目前这些资源仅仅用于landing
    zone管理，并不含有客户业务系统的workload资源。

![电脑萤幕的截图
描述已自动生成](./media/media/image26.png)

![图形用户界面, 文本, 应用程序, 电子邮件
描述已自动生成](./media/media/image27.png)

15. 每个部署的readme文件在完成了相应部分的部署操作后，都会提示下一步操作，按readme中指引的Next
    Step完成之后的操作任务。当完成level2后，所需的企业级landing
    zone部署基本就绪了。Partner和客户可以在这之上继续使用Azrue
    Terraform部署应用worklaod。

![文本
描述已自动生成](./media/media/image28.png)

16. 上面实验中，通过Azure Terraform Rover完成了Landing Zone的部署，即：

> Level 0： Lauchpad，
>
> Level 1 ：管理、身份，然后是 alz。完成第 1 级后，
>
> Level 2: Azure 订阅自动售货机 (asvm) 和身份，以及使用虚拟 WAN
> 说明的连接组件。

## 考虑

接下来，Partner可以通过生成的tfvar变量及rover命令近一步理解Azure
Terraform Provider/Azure Terraform Module/Rover/Rover
Ignite逻辑关系，并尝试部署你客户业务场景中所需的上层应用workload，比如
IaaS（VM，存储等），PaaS（Mysql，Web Apps等)。

近一步学习和使用Azure Landing Zone for Terraform还可以参考如下资源：

1.  [Azure Landing
    Zone](https://docs.microsoft.com/zh-cn/azure/cloud-adoption-framework/ready/landing-zone/)

2.  [Terraform语言](https://www.terraform.io/language)

3.  [Cloud Adoption Framework for Terraform Landing
    zones](https://aztfmod.github.io/documentation/)

4.  [CAF Landing Zone
    Starter](https://github.com/Azure/caf-terraform-landingzones-starter)

5.  [Azure - Terraform landing zones module and
    solutions](https://github.com/aztfmod/terraform-azurerm-caf)
