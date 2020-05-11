# Observable 객체
- Observable 객체는 시간이 지남에 따라 반복적으로 변하는 데이터 값인 동적 데이터를 래핑하는 데 사용될 때 강력하다는 특징이 있다.

Observable 객체의 경우엔 두 개 이상의 화면이 동일한 객체를 참조할 때, 아래와 같은 방식으로 참조체를 전달해야 한다.
<pre><code>
  **ContentView.swift**
  NavigationLink(destination:
             SecondView(timerData: self.timerData)){
             Text("Next Screen")
  }

  **SecondView.swift**
  struct SecondView_Previews: PreviewProvider {
    static var previews: some View {
        SecondView(timerData: TimerData())
    }
}
</code></pre>

이렇게 작성 된 경우 서로 같은 참조체를 공유할 수는 있지만 참조체를 전달해야하는 수고가 생긴다.

이러한 문제를 해결하기 위해서는 Environment 객체를 사용하면 해결된다.


# Environment 객체
- Environment는 전반적으로 Observable 객체와 유사하지만 다른 뷰로 데이터를 전달하지 않아도 다른 뷰들이 접근할 수 있게 된다.

Observable 객체로 구성했던 것에서 참조체를 전달하는 부분을 없애고 SceneDelgate.swift 파일을 수정하여 최상위(루트) 화면이 생성될 때 TimerData 객체가
Environment 객체에 추가되도록 수정해야 한다.

<pre><code>
  **SceneDelgate.swift**
  //루트 화면이 생성될 때 TimerData 객체가 Environment 객체에 추가되도록 설정
        let contentView = ContentView()
        
        let timerData = TimerData()
        
        if let windowScene = scene as? UIWindowScene {
            let window = UIWindow(windowScene: windowScene)
            
            window.rootViewController = UIHostingController(rootView: contentView.environmentObject(timerData))
            
            self.window = window
            window.makeKeyAndVisible()
        }
</code></pre>







