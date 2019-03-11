# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="cee88-101">Ausführen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="cee88-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cee88-102">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="cee88-102">Prerequisites</span></span>

<span data-ttu-id="cee88-103">Zum Ausführen des abgeschlossenen Projekts in diesem Ordner benötigen Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="cee88-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="cee88-104">[Visual Studio](https://visualstudio.microsoft.com/vs/) auf dem Entwicklungscomputer installiert.</span><span class="sxs-lookup"><span data-stu-id="cee88-104">[Visual Studio](https://visualstudio.microsoft.com/vs/) installed on your development machine.</span></span> <span data-ttu-id="cee88-105">Wenn Sie nicht über Visual Studio verfügen, besuchen Sie den vorherigen Link für Downloadoptionen.</span><span class="sxs-lookup"><span data-stu-id="cee88-105">If you do not have Visual Studio, visit the previous link for download options.</span></span> <span data-ttu-id="cee88-106">(**Hinweis:** dieses Lernprogramm wurde mit der Version 15,81 von Visual Studio 2017 geschrieben.</span><span class="sxs-lookup"><span data-stu-id="cee88-106">(**Note:** This tutorial was written with Visual Studio 2017 version 15.81.</span></span> <span data-ttu-id="cee88-107">Die Schritte in diesem Handbuch funktionieren möglicherweise mit anderen Versionen, die jedoch nicht getestet wurden.)</span><span class="sxs-lookup"><span data-stu-id="cee88-107">The steps in this guide may work with other versions, but that has not been tested.)</span></span>
- <span data-ttu-id="cee88-108">Entweder ein persönliches Microsoft-Konto mit einem Postfach auf Outlook.com oder ein Microsoft-Geschäfts-oder Schulkonto.</span><span class="sxs-lookup"><span data-stu-id="cee88-108">Either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span>

<span data-ttu-id="cee88-109">Wenn Sie nicht über ein Microsoft-Konto verfügen, gibt es eine Reihe von Optionen, um ein kostenloses Konto zu erhalten:</span><span class="sxs-lookup"><span data-stu-id="cee88-109">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="cee88-110">Sie können [sich für ein neues persönliches Microsoft-Konto registrieren](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span><span class="sxs-lookup"><span data-stu-id="cee88-110">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="cee88-111">Sie können [sich für das office 365-Entwicklerprogramm anmelden](https://developer.microsoft.com/office/dev-program) , um ein kostenloses Office 365-Abonnement zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="cee88-111">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-native-application-with-the-application-registration-portal"></a><span data-ttu-id="cee88-112">Registrieren einer systemeigenen Anwendung im Anwendungs Registrierungs Portal</span><span class="sxs-lookup"><span data-stu-id="cee88-112">Register a native application with the Application Registration Portal</span></span>

1. <span data-ttu-id="cee88-113">Öffnen Sie einen Browser, und navigieren Sie zum [Anwendungs Registrierungs Portal](https://apps.dev.microsoft.com) und melden Sie sich über ein **persönliches Konto** (aka: Microsoft-Konto) oder ein Geschäfts- **oder Schulkonto**an.</span><span class="sxs-lookup"><span data-stu-id="cee88-113">Open a browser and navigate to the [Application Registration Portal](https://apps.dev.microsoft.com) and login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="cee88-114">Wählen Sie oben auf der Seite **eine APP hinzufügen** aus.</span><span class="sxs-lookup"><span data-stu-id="cee88-114">Select **Add an app** at the top of the page.</span></span>

    > <span data-ttu-id="cee88-115">**Hinweis:** Wenn auf der Seite mehr als eine Schaltfläche **app hinzufügen** angezeigt wird, wählen Sie diejenige aus, die der Liste **konvergierter apps** entspricht.</span><span class="sxs-lookup"><span data-stu-id="cee88-115">**Note:** If you see more than one **Add an app** button on the page, select the one that corresponds to the **Converged apps** list.</span></span>

1. <span data-ttu-id="cee88-116">Legen Sie auf der Seite **Anwendung registrieren** den **Anwendungsnamen** auf **UWP Graph-Lernprogramm** fest, und wählen Sie **Erstellen**aus.</span><span class="sxs-lookup"><span data-stu-id="cee88-116">On the **Register your application** page, set the **Application Name** to **UWP Graph Tutorial** and select **Create**.</span></span>

    ![Screenshot des Erstellens einer neuen app in der APP-Registrierungs Portal-Website](../../../Images/arp-create-app-01.png)

1. <span data-ttu-id="cee88-118">Kopieren Sie auf der Seite **UWP Graph Tutorial-Registrierung** im Abschnitt **Eigenschaften** die **Anwendungs-ID** , so wie Sie Sie später benötigen.</span><span class="sxs-lookup"><span data-stu-id="cee88-118">On the **UWP Graph Tutorial Registration** page, under the **Properties** section, copy the **Application Id** as you will need it later.</span></span>

    ![Screenshot der neu erstellten Anwendungs-ID](../../../Images/arp-create-app-02.png)

1. <span data-ttu-id="cee88-120">Scrollen Sie nach unten zum Abschnitt **Plattformen** .</span><span class="sxs-lookup"><span data-stu-id="cee88-120">Scroll down to the **Platforms** section.</span></span>

    1. <span data-ttu-id="cee88-121">Wählen Sie **Plattform hinzufügen**aus.</span><span class="sxs-lookup"><span data-stu-id="cee88-121">Select **Add Platform**.</span></span>
    1. <span data-ttu-id="cee88-122">Wählen Sie im Dialogfeld **Plattform hinzufügen** die Option **systemeigene Anwendung**aus.</span><span class="sxs-lookup"><span data-stu-id="cee88-122">In the **Add Platform** dialog, select **Native Application**.</span></span>

        ![Screenshot Erstellen einer Plattform für die APP](../../../Images/arp-create-app-03.png)

1. <span data-ttu-id="cee88-124">Scrollen Sie zum unteren Rand der Seite, und wählen Sie **Speichern**aus.</span><span class="sxs-lookup"><span data-stu-id="cee88-124">Scroll to the bottom of the page and select **Save**.</span></span>

## <a name="configure-the-sample"></a><span data-ttu-id="cee88-125">Konfigurieren des Beispiels</span><span class="sxs-lookup"><span data-stu-id="cee88-125">Configure the sample</span></span>

1. <span data-ttu-id="cee88-126">Benennen Sie `OAuth.resw.example` die Datei `OAuth.resw`in um.</span><span class="sxs-lookup"><span data-stu-id="cee88-126">Rename the `OAuth.resw.example` file to `OAuth.resw`.</span></span>
1. <span data-ttu-id="cee88-127">Öffnen `graph-tutorial.sln` Sie in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cee88-127">Open `graph-tutorial.sln` in Visual Studio.</span></span>
1. <span data-ttu-id="cee88-128">Bearbeiten Sie `OAuth.resw` die Datei in Visual Studio. Ersetzen `YOUR_APP_ID_HERE` Sie durch die **Anwendungs-ID** , die Sie im App-Registrierungs Portal erhalten haben.</span><span class="sxs-lookup"><span data-stu-id="cee88-128">Edit the `OAuth.resw` file in visual studio.Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>
1. <span data-ttu-id="cee88-129">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf die **Graph-Tutorial-** Lösung, und wählen Sie **NuGet-Pakete wiederherstellen**.</span><span class="sxs-lookup"><span data-stu-id="cee88-129">In Solution Explorer, right-click the **graph-tutorial** solution and choose **Restore NuGet Packages**.</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="cee88-130">Ausführen des Beispiels</span><span class="sxs-lookup"><span data-stu-id="cee88-130">Run the sample</span></span>

<span data-ttu-id="cee88-131">Drücken Sie in Visual Studio **F5** , oder klicken Sie auf **Debuggen > Start Debugging**.</span><span class="sxs-lookup"><span data-stu-id="cee88-131">In Visual Studio, press **F5** or choose **Debug > Start Debugging**.</span></span>