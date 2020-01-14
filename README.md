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

           let noOfCellsInRow = 3 // set here your desigred number
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

<h5> Step 1 Create a class </h5>

```Swift
import UIKit

class ColumnFlowLayout: UICollectionViewFlowLayout {

    let cellsPerRow: Int

    init(cellsPerRow: Int, minimumInteritemSpacing: CGFloat = 0, minimumLineSpacing: CGFloat = 0, sectionInset: UIEdgeInsets = .zero) {
        self.cellsPerRow = cellsPerRow
        super.init()

        self.minimumInteritemSpacing = minimumInteritemSpacing
        self.minimumLineSpacing = minimumLineSpacing
        self.sectionInset = sectionInset
    }

    required init?(coder aDecoder: NSCoder) {
        fatalError("init(coder:) has not been implemented")
    }

    override func prepare() {
        super.prepare()

        guard let collectionView = collectionView else { return }
        let marginsAndInsets = sectionInset.left + sectionInset.right + collectionView.safeAreaInsets.left + collectionView.safeAreaInsets.right + minimumInteritemSpacing * CGFloat(cellsPerRow - 1)
        let itemWidth = ((collectionView.bounds.size.width - marginsAndInsets) / CGFloat(cellsPerRow)).rounded(.down)
        itemSize = CGSize(width: itemWidth, height: itemWidth)
    }

    override func invalidationContext(forBoundsChange newBounds: CGRect) -> UICollectionViewLayoutInvalidationContext {
        let context = super.invalidationContext(forBoundsChange: newBounds) as! UICollectionViewFlowLayoutInvalidationContext
        context.invalidateFlowLayoutDelegateMetrics = newBounds.size != collectionView?.bounds.size
        return context
    }

}

```


<h5> Step 2 Use it in your Controller </h5>

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


<h2> Best Swift Blogs/Websites:  </h2>

<h4> 1.theswiftdev https://theswiftdev.com/  </h4>
<h4> 2.Easy Google Maps Setup Tutorial— Swift 5 https://medium.com/better-programming/easy-google-maps-setup-tutorial-swift-4-f6d5c093817e  </h4>

