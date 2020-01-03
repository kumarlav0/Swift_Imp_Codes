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


<h3> 3. How to set Navigation Color Clear or Hide Navigation bar.  </h3>

```Swift

 //to make transparent to navigation controller
   self.navigationController?.navigationBar.setBackgroundImage(UIImage(), for: UIBarMetrics.default)
   self.navigationController?.navigationBar.shadowImage = UIImage()
   self.navigationController?.navigationBar.isTranslucent = true

```

<h3> 4. How to set CollectionView number of Items per Row.  </h3>


```Swift
// use UICollectionViewDelegateFlowLayout .   this Important Protocol
class ViewController: UIViewController, UICollectionViewDataSource, UICollectionViewDelegate, UICollectionViewDelegateFlowLayout
{
@IBOutlet weak var galleryCollectionView: UICollectionView!

      // use this method
       func collectionView(_ collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, sizeForItemAt indexPath: IndexPath) -> CGSize 
       {

           let noOfCellsInRow = 3
           let flowLayout = collectionViewLayout as! UICollectionViewFlowLayout
           let totalSpace = flowLayout.sectionInset.left
                             + flowLayout.sectionInset.right
                             + (flowLayout.minimumInteritemSpacing * CGFloat(noOfCellsInRow - 1))
           let size = Int((collectionView.bounds.width - totalSpace) / CGFloat(noOfCellsInRow))
           return CGSize(width: size, height: size)
       }
 
 
}   

```

<h3> 5. How to set CollectionView number of Items per Row.  </h3>

```Swift

import UIKit

class CollectionViewController: UICollectionViewController {

    let columnLayout = ColumnFlowLayout(
        cellsPerRow: 5,
        minimumInteritemSpacing: 10,
        minimumLineSpacing: 10,
        sectionInset: UIEdgeInsets(top: 10, left: 10, bottom: 10, right: 10)
    )

    override func viewDidLoad() {
        super.viewDidLoad()

        collectionView?.collectionViewLayout = columnLayout
        collectionView?.contentInsetAdjustmentBehavior = .always
        collectionView?.register(UICollectionViewCell.self, forCellWithReuseIdentifier: "Cell")
    }

    override func collectionView(_ collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {
        return 59
    }

    override func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell {
        let cell = collectionView.dequeueReusableCell(withReuseIdentifier: "Cell", for: indexPath)
        cell.backgroundColor = UIColor.orange
        return cell
    }

}

```
<h5> CollectionView items Example:  </h5>
<img src="https://i.stack.imgur.com/O7oAX.png" width="889" height="431">
