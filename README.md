# adMob Interstitial Ads without button click

## 구글 애드몹 전면 광고입니다.
#### 애드몹 전면 광고를 버튼 없이 특정 카운트인 경우 반복해서 나타냅니다. 
#### 제 경우는 광고 제거 버전을 구입하지 않으면서 페이지 카운트가 4의 배수인 경우로 되어있습니다만 경우에 따라서 랜덤한 카운트로 변경할 수 있습니다.


### Constants.swift 파일 생성
    
    // 본인의 디바이스 UUID, 애드몹의 어플리케이션 아이디, 광고 유닛 아이디 등을 넣습니다.
    let adMobMyDeviceUUID = ""
    let adMobApplicationId = ""
    //let adMobBannerAdUnitId = ""
    let adMobInterstitialAdUnitId = ""

### createAndLoadInterstital() 생성
    @objc fileprivate func createAndLoadInterstital() {
        interstitial = GADInterstitial(adUnitID : adMobInterstitialAdUnitId)
        let request = GADRequest()
        request.testDevices = [kGADSimulatorID, adMobMyDeviceUUID]
        interstitial.load(request)
    }

### checkAd() 생성
    @objc func checkAd() {
        if interstitial.isReady {
            interstitial.present(fromRootViewController: self)
            createAndLoadInterstital()
        } else {
            print("광고 준비중")
        }
    }
    
### 뷰컨트롤러에 interstitial 생성
    fileprivate var interstitial: GADInterstitial!
 
### 뷰디드로드에 interstitial 생성, createAndLoadInterstital()호출, 노티피케이션 옵저버 설정
    createAndLoadInterstital()
    NotificationCenter.default.addObserver(self, selector: #selector(self.checkAd), name: NSNotification.Name(rawValue: "loadAndShow"), object: nil)

### 원하는 액션이 있는 부분에 노티피케이션 포스트 설정
    // 광고를 구입했고 페이지 카운트가 4의 배수가 되는 경우 광고 실행
    if isBuyRemoveAds == false {
      if readPageCount % 4 == 0 {
        NotificationCenter.default.post(name: NSNotification.Name(rawValue: "loadAndShow"), object: nil)
      }
    }
    readPageCount += 1
