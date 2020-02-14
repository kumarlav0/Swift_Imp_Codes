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



<h2> Load XIB as UIView in UIViewController:  </h2>


```Swift
Step1. Create Xib file and design according to your wish.
Step2. Create UIView File with the same name as of Xib.
Like: TestView.swift and TestView.xib


class TestView: UIView {
    
    @IBOutlet var contentView: UIView!
    
    @IBOutlet weak var profileImgView: UIImageView!
    @IBOutlet weak var userNameLabel: UILabel!
    @IBOutlet weak var subscribeButton: UIButton!
    
    override init(frame: CGRect) {
        super.init(frame:frame)
        commonInit()
    }
    
    required init?(coder aDecoder: NSCoder) {
        super.init(coder:aDecoder)
        commonInit()
    }
    
    // This is a very imp method to remember 
   private func commonInit()
    {
        Bundle.main.loadNibNamed("TestView", owner: self, options: nil)
        addSubview(contentView)
        contentView.frame = self.bounds
        contentView.autoresizingMask = [.flexibleHeight, .flexibleWidth]
    }
    
 
    
}

// Used in ViewController
import UIKit

class ViewController: UIViewController {

    @IBOutlet weak var testView: TestView!
    
    
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.
        testView.profileImgView.image = #imageLiteral(resourceName: "download")
        testView.userNameLabel.text! = "New text on label"
        
        testView.subscribeButton.addTarget(self, action: #selector(subscribe), for: .touchUpInside)
    }

    @objc func subscribe(){
        print("Clicked.....")
    }
    
    
}

```



<h2> Best Swift Blogs/Websites:  </h2>

<h4> 1.theswiftdev https://theswiftdev.com/  </h4>
<h4> 2.Easy Google Maps Setup Tutorial— Swift 5 https://medium.com/better-programming/easy-google-maps-setup-tutorial-swift-4-f6d5c093817e  </h4>


<h2> InApp Purchase:  </h2>

```Swift

import UIKit
import StoreKit


/*                                       PACKAGE_ID       DURATION
 1. SILVER
 1.1  SILVER 10 USERS = ₹ 299/-             1               1 MONTH
 1.2  SILVER 20 USERS = ₹ 549/-             2               1 MONTH
 1.3  SILVER 30 USERS = ₹ 799/-             3               1 MONTH
 
 2. GOLD
 2.1  GOLD 10 USERS = ₹ 1499/-              4               6 MONTHS
 2.2  GOLD 20 USERS = ₹ 2799/-              5               6 MONTHS
 2.3  GOLD 30 USERS = ₹ 3999/-              6               6 MONTHS
 
 3. PATINUM
 3.1  PATINUM 10 USERS = ₹ 2699/-           7               12 MONTHS
 3.2  PATINUM 20 USERS = ₹ 4999/-           8               12 MONTHS
 3.3  PATINUM 30 USERS = ₹ 6900/-           9               12 MONTHS
 
 4. FREE TRIAL                              10              12 MONTHS
 */



class SubscriptionsViewController: UIViewController,SKProductsRequestDelegate,SKPaymentTransactionObserver {
    
    
    @IBOutlet weak var PackagesSegmentControl: UISegmentedControl!
    @IBOutlet weak var UsersSegmentControl: UISegmentedControl!
    
    @IBOutlet weak var DurationLabel: UILabel!
    @IBOutlet weak var Pricelabel: UILabel!
    
    @IBOutlet weak var BuyView: UIView!
    @IBOutlet weak var BuyNowButton: UIButton!
    
   
    var package_id = "1"
    
    
//   var products: [SKProduct] = []  //
//
    var selectedProductIndex: Int!
    
    var transactionInProgress = false
    
    var productIDs: Array<String!> = []
    
    var productsArray: Array<SKProduct!> = []  // From App Store
    
    
    override func viewDidLoad() {
        super.viewDidLoad()
        BuyView.layer.cornerRadius = 5
        BuyNowButton.isUserInteractionEnabled = false
        BuyNowButton.layer.cornerRadius = 2.0
        self.setProductId()
        self.requestProductInfo()
        
        SKPaymentQueue.default().add(self)
        
    }
    
    override func viewWillAppear(_ animated: Bool) {
        
    }
    
    
    // MARK:- Send Request to App Store
    func requestProductInfo() {
        if SKPaymentQueue.canMakePayments() {
            let productIdentifiers = NSSet(array: productIDs)
            let productRequest = SKProductsRequest(productIdentifiers: productIdentifiers as! Set<String>)
            
            productRequest.delegate = self
            productRequest.start()
             print("Can perform In App Purchases.............")
        }
        else {
            print("Cannot perform In App Purchases.")
        }
    }
    
    
    // MARK:- Get Response from App Store for Product_id's
    func productsRequest(_ request: SKProductsRequest, didReceive response: SKProductsResponse) {
        if response.products.count != 0 {
            for product in response.products {
                productsArray.append(product as! SKProduct)
            }
            
             BuyNowButton.backgroundColor = #colorLiteral(red: 0.8509803922, green: 0.2862745098, blue: 0.03529411765, alpha: 1)
             BuyNowButton.isUserInteractionEnabled = true
            print("Products::::",productsArray)
            for i in 0..<productsArray.count
            {
                print("Plan\(i+1)::",productsArray[i]?.localizedTitle," Price::",productsArray[i]?.price,"\n Product ID:::",productsArray[i]?.productIdentifier)
                // product.localizedTitle
            }
            
            
        }
        else {
            print("There are no products.")
            
        }
        
        if response.invalidProductIdentifiers.count != 0 {
            print("invalidProductIdentifiers::::::::::::",response.invalidProductIdentifiers.description)
        }
        
        
        
    }
    
    
    // MARK:- Transaction Completed
    func paymentQueue(_ queue: SKPaymentQueue, updatedTransactions transactions: [SKPaymentTransaction]) {
       
        for transaction in transactions as! [SKPaymentTransaction] {
            switch transaction.transactionState {
            case SKPaymentTransactionState.purchased:
                print("Transaction completed successfully.")
               SKPaymentQueue.default().finishTransaction(transaction)
                transactionInProgress = false
                self.update_customer_package_id()
            case SKPaymentTransactionState.failed:
                print("Transaction Failed");
                Global.showAlertMessageWithOkButtonAndTitle("Transaction Failed", andMessage: "Please try again.")
                SKPaymentQueue.default().finishTransaction(transaction)
                transactionInProgress = false
                
            default:
                print(transaction.transactionState.rawValue)
            }
        }
    }
    

    
    @objc func restoreTapped(_ sender: AnyObject) {
        RazeFaceProducts.store.restorePurchases()
    }
    
    
    
    // MARK:- Buy Subscriptions
    @IBAction func BuyNowAction(_ sender: Any) {
         print("Users Segmemt index :\(UsersSegmentControl.selectedSegmentIndex )..... Packages::: Segment index \(PackagesSegmentControl.selectedSegmentIndex)")
        
        if PackagesSegmentControl.selectedSegmentIndex == 0  // For SILVER
        {
                            if UsersSegmentControl.selectedSegmentIndex == 0  // 10 USER 299/-
                            {
                                let payment = SKPayment(product: self.productsArray[6] as! SKProduct)
                                print("Silver:: 10, 299 ==",self.productsArray[6]!.price)
                                self.package_id = "1"
                                SKPaymentQueue.default().add(payment)
                                self.transactionInProgress = true
                            }
                            else if UsersSegmentControl.selectedSegmentIndex == 1  // 20 USER 549/-
                            {
                                let payment = SKPayment(product: self.productsArray[7] as! SKProduct)
                                print("Siver:: 20, 549 ==",self.productsArray[7]!.price)
                                 self.package_id = "2"
                                SKPaymentQueue.default().add(payment)
                                self.transactionInProgress = true
                            }
                            else if UsersSegmentControl.selectedSegmentIndex == 2 // 30 USERS 799
                            {
                                let payment = SKPayment(product: self.productsArray[8] as! SKProduct)
                                print("Silver:: 30, 799 ==",self.productsArray[8]!.price)
                                 self.package_id = "3"
                                SKPaymentQueue.default().add(payment)
                                self.transactionInProgress = true
                            }
        }
        else  if PackagesSegmentControl.selectedSegmentIndex == 1  // For GOLD
        {
                            if UsersSegmentControl.selectedSegmentIndex == 0  // 10 USERS 1499/-
                            {
                                let payment = SKPayment(product: self.productsArray[0] as! SKProduct)
                                print("Gold:: 10, 1499 ==",self.productsArray[0]!.price)
                                 self.package_id = "4"
                                SKPaymentQueue.default().add(payment)
                                self.transactionInProgress = true
                            }
                            else if UsersSegmentControl.selectedSegmentIndex == 1  // 20 USERS 2799/-
                            {
                                let payment = SKPayment(product: self.productsArray[1] as! SKProduct)
                                print("Gold:: 20, 2799 ==",self.productsArray[1]!.price)
                                 self.package_id = "5"
                                SKPaymentQueue.default().add(payment)
                                self.transactionInProgress = true
                            }
                            else if UsersSegmentControl.selectedSegmentIndex == 2  // 3999/-
                            {
                                let payment = SKPayment(product: self.productsArray[2] as! SKProduct)
                                print("Glod:: 30, 3999 ==",self.productsArray[2]!.price)
                                 self.package_id = "6"
                                SKPaymentQueue.default().add(payment)
                                self.transactionInProgress = true
                            }
        }
        else  if PackagesSegmentControl.selectedSegmentIndex == 2  // For Platinum
        {
                        if UsersSegmentControl.selectedSegmentIndex == 0  // 10 USERS 2699/-
                        {
                            let payment = SKPayment(product: self.productsArray[3] as! SKProduct)
                            print("Platinum:: 10, 2699 ==",self.productsArray[3]!.price)
                             self.package_id = "7"
                            SKPaymentQueue.default().add(payment)
                            self.transactionInProgress = true
                        }
                        else if UsersSegmentControl.selectedSegmentIndex == 1  // 20 USERS 4999/-
                        {
                            let payment = SKPayment(product: self.productsArray[4] as! SKProduct)
                            print("Platinum:: 20, 4999 ==",self.productsArray[4]!.price)
                             self.package_id = "8"
                            SKPaymentQueue.default().add(payment)
                            self.transactionInProgress = true
                        }
                        else if UsersSegmentControl.selectedSegmentIndex == 2 // 30 USERS 6990/-
                        {
                            let payment = SKPayment(product: self.productsArray[5] as! SKProduct)
                             self.package_id = "9"
                            print("Platinum:: 30, 6900 ==",self.productsArray[5]!.price)
                            SKPaymentQueue.default().add(payment)
                            self.transactionInProgress = true
                        }
        }
        
        
    }
    
    
    
    
    // MARK: - Package Selection
    
    @IBAction func PackageSelection(_ sender: UISegmentedControl) {
        if PackagesSegmentControl.selectedSegmentIndex == 0
        {
            print("Users Segmemt index :\(UsersSegmentControl.selectedSegmentIndex )..... Packages::: Segment index \(PackagesSegmentControl.selectedSegmentIndex)")
            
            if UsersSegmentControl.selectedSegmentIndex == 0{
                print("0000000")
                //this is for silver
                self.DurationLabel.text! = "1 Month"
                self.Pricelabel.text! = "INR 299/-"
            }else if UsersSegmentControl.selectedSegmentIndex == 1{
                print("11111111111111")
                //this is for silver
                self.DurationLabel.text! = "1 Month"
                self.Pricelabel.text! = "INR 549/-"
            }else if UsersSegmentControl.selectedSegmentIndex == 2{
                print("222222222222222222222")
                //this is for silver
                self.DurationLabel.text! = "1 Month"
                self.Pricelabel.text! = "INR 799/-"
            }
        }
        else  if PackagesSegmentControl.selectedSegmentIndex == 1
        {
            if UsersSegmentControl.selectedSegmentIndex == 0{
                self.DurationLabel.text! = "6 Months"
                self.Pricelabel.text! = "INR 1499/-"
                //this is for Gold
            }else if UsersSegmentControl.selectedSegmentIndex == 1{
                
                self.DurationLabel.text! = "6 Months"
                self.Pricelabel.text! = "INR 2799/-"
                //this is for Gold
            }else if UsersSegmentControl.selectedSegmentIndex == 2{
                //this is for Gold
                self.DurationLabel.text! = "6 Months"
                self.Pricelabel.text! = "INR 3999/-"
            }
        }
        else  if PackagesSegmentControl.selectedSegmentIndex == 2
        {
            if UsersSegmentControl.selectedSegmentIndex == 0{
                self.DurationLabel.text! = "12 Months"
                self.Pricelabel.text! = "INR 2699/-"
                //this is for Platinum
            }else if UsersSegmentControl.selectedSegmentIndex == 1{
                //this is for Platinum
                self.DurationLabel.text! = "12 Months"
                self.Pricelabel.text! = "INR 4999/-"
            }
            else if UsersSegmentControl.selectedSegmentIndex == 2
            {
                self.DurationLabel.text! = "12 Months"
                self.Pricelabel.text! = "INR 6990/-"
                //this is for Platinum
            }
        }
        
    }
    
    
    
    // MARK:- User Selection
    @IBAction func UserSelection(_ sender: UISegmentedControl) {
        
        if PackagesSegmentControl.selectedSegmentIndex == 0
        {
            print("Users Segmemt index :\(UsersSegmentControl.selectedSegmentIndex )..... Packages::: Segment index \(PackagesSegmentControl.selectedSegmentIndex)")
            
            if UsersSegmentControl.selectedSegmentIndex == 0{
                print("0000000")
                //this is for silver
                self.DurationLabel.text! = "1 Month"
                self.Pricelabel.text! = "INR 299/-"
            }else if UsersSegmentControl.selectedSegmentIndex == 1{
                print("11111111111111")
                //this is for Gold
                self.DurationLabel.text! = "1 Month"
                self.Pricelabel.text! = "INR 549/-"
            }else if UsersSegmentControl.selectedSegmentIndex == 2{
                print("222222222222222222222")
                //this is for Platinum
                self.DurationLabel.text! = "1 Month"
                self.Pricelabel.text! = "INR 799/-"
            }
        }
        else  if PackagesSegmentControl.selectedSegmentIndex == 1
        {
            if UsersSegmentControl.selectedSegmentIndex == 0{
                self.DurationLabel.text! = "6 Months"
                self.Pricelabel.text! = "INR 1499/-"
                //this is for silver
            }else if UsersSegmentControl.selectedSegmentIndex == 1{
                
                self.DurationLabel.text! = "6 Months"
                self.Pricelabel.text! = "INR 2799/-"
                //this is for Gold
            }else if UsersSegmentControl.selectedSegmentIndex == 2{
                //this is for Platinum
                self.DurationLabel.text! = "6 Months"
                self.Pricelabel.text! = "INR 3999/-"
            }
        }
        else  if PackagesSegmentControl.selectedSegmentIndex == 2
        {
            if UsersSegmentControl.selectedSegmentIndex == 0{
                self.DurationLabel.text! = "12 Months"
                self.Pricelabel.text! = "INR 2699/-"
                //this is for silver
            }else if UsersSegmentControl.selectedSegmentIndex == 1{
                //this is for Gold
                self.DurationLabel.text! = "12 Months"
                self.Pricelabel.text! = "INR 4999/-"
            }
            else if UsersSegmentControl.selectedSegmentIndex == 2
            {
                self.DurationLabel.text! = "12 Months"
                self.Pricelabel.text! = "INR 6990/-"
                //this is for Platinum
            }
        }
        
    }
    
   
  
    
    
    func setProductId()
    {
        // SILVER
         productIDs.append("com.Empro.Silver10UserOneMonth")   // 299 INR
         productIDs.append("com.Empro.Silver20UserOneMonth")   // 549 INR
         productIDs.append("com.Empro.Silver30UserOneMonth")   // 799 INR
        
        // GOLD
         productIDs.append("com.Empro.Gold10UserSixMonth")     // 1499 INR
         productIDs.append("com.Empro.Gold20UserSixMonth")     // 2799 INR
         productIDs.append("com.Empro.Gold30UserSixMonth")     // 3999 INR
        
        // PLATINUM
         productIDs.append("com.Empro.Platinum10UserOneYear")  // 2699 INR
         productIDs.append("com.Empro.Platinum20UserOneYear")  // 4999 INR
         productIDs.append("com.Empro.Platinum30UserOneYear")  // 6990 INR
        
    }
    
    
    func update_customer_package_id()
    {
        var dictPost:[String: AnyObject]!
        dictPost = ["customer_id":UserDefaults.standard.value(forKey: "customer_id") as AnyObject,"package_id":self.package_id as AnyObject]
        print("<<<<<<<<<customer_id>>>>>>>>:",dictPost)
        print("emp_id")
        WebHelper.requestPostUrl(strURL: GlobalConstant.update_customer_package_id, Dictionary: dictPost, Success:{
            success in
            print(":::::::::::::Success:::::::::",success)
           
            let resultString = success.object(forKey: "status") as! NSString
         
            // Result fail
            if resultString == "0"
            {
                print("Result is:00000000000000")
            }
            // Result success
            else if resultString == "1"
            {
                print("Data:111111111111111111")
            }  // Result nil
            else
            {
                //Global.showAlertMessageWithOkButtonAndTitle(GlobalConstant.APP_NAME, andMessage: GlobalConstant.InternalServerErrorMessage)
            }
        }, Failure: {
            failure in
            //Global.showAlertMessageWithOkButtonAndTitle(GlobalConstant.APP_NAME, andMessage: failure.localizedDescription)
        })
    } //End of API method Calling
    
    
    
    
    
    
}



```
