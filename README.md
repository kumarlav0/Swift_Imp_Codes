# Some Important Codes for Swift.

```Swift
1. How to jump to Navigation Controller screen from a Non- Navigation Controller Screen.
   Like: from LoginViewController to HomeViewController Screen

@IBAction func goToNext(_ sender: Any) {
  
    let homeVC = storyboard?.instantiateViewController(withIdentifier: "HomeViewController") as! HomeViewController
    let navigationController = UINavigationController(rootViewController: homeVC)
        navigationController.modalPresentationStyle = .fullScreen  // if you are using iOS 13 or more else this line of code is not required.
        self.present(navigationController, animated: true, completion: nil)
        
    }



```
