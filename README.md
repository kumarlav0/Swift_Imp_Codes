# Some Important Codes for Swift.

<h3> 1. How to jump to Navigation Controller screen from a Non- Navigation Controller Screen.
   Like: from LoginViewController to HomeViewController Screen  </h3>
   
   
```Swift

@IBAction func goToNext(_ sender: Any) {
  
    let homeVC = storyboard?.instantiateViewController(withIdentifier: "HomeViewController") as! HomeViewController
    let navigationController = UINavigationController(rootViewController: homeVC)
        navigationController.modalPresentationStyle = .fullScreen  // if you are using iOS 13 or more else this line of code is not required.
        self.present(navigationController, animated: true, completion: nil)
        
    }

```

<h3> 2. How to set Image in Navigation Title as a logo.  </h3>

```Swift

 override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.
        self.addNavBarImage()
    }

//MARK:- Mathod to set Image in Navigation title.
 func addNavBarImage() {
        let navController = navigationController!
        let image = UIImage(named: "company_logo") //Your logo image name should be here
        let imageView = UIImageView(image: image)
        let bannerWidth = navController.navigationBar.frame.size.width
        let bannerHeight = navController.navigationBar.frame.size.height
        let bannerX = bannerWidth / 2 - (image?.size.width)! / 2
        let bannerY = bannerHeight / 2 - (image?.size.height)! / 2
        imageView.frame = CGRect(x: bannerX, y: bannerY, width: bannerWidth, height: bannerHeight)
        imageView.contentMode = .scaleAspectFit
        navigationItem.titleView = imageView
    }

```

<h5> Navigation Title as a logo Example:  </h5>
<img src="https://i.stack.imgur.com/JeZZY.png" width="400" height="200">


