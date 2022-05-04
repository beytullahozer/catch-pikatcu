## Definition
<br>

## Catch Pikatcu
<br>

<br> It is a race against time. When clicked, the number of scores increases and random pictures change depending on the time.

<br> The highest score at the end of the time is recorded as the high score. When the game is restarted, the high score is saved in the application.

<p align="center"> <img src="https://user-images.githubusercontent.com/88663603/137084358-044b94fd-2df3-4d19-acce-658ae874bc7a.png" width="300" height="600" /> <img src="https://user-images.githubusercontent.com/88663603/137084361-13d30a05-6b26-4db6-a9f3-30d0d3b74346.png" width="300" height="600" />
<p align="center"> <img src="https://user-images.githubusercontent.com/88663603/137084362-2ce422e2-f017-45aa-a4e5-d6a4d5d7955a.png" width="300" height="600" /> <img src="https://user-images.githubusercontent.com/88663603/137084432-843d4350-479c-491b-8a04-b7875cef12e9.png" width="300" height="600" />


<br>
    
import UIKit

class ViewController: UIViewController {

    // Variables
    var score = 0
    var counter = 0
    var time = Timer()
    var hideTimer = Timer()
    var imageArray = [UIImageView] ()
    var highScore = 0
    
    // Views
    @IBOutlet weak var timeLabel: UILabel!
    @IBOutlet weak var scoreLabel: UILabel!
    @IBOutlet weak var highScoreLabel: UILabel!
    @IBOutlet weak var image1: UIImageView!
    @IBOutlet weak var image2: UIImageView!
    @IBOutlet weak var image3: UIImageView!
    @IBOutlet weak var image4: UIImageView!
    @IBOutlet weak var image5: UIImageView!
    @IBOutlet weak var image6: UIImageView!
    @IBOutlet weak var image7: UIImageView!
    @IBOutlet weak var image8: UIImageView!
    @IBOutlet weak var image9: UIImageView!
    
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        scoreLabel.text = "Score: \(score)"
        
        // High Score Check
        let storedHighScore = UserDefaults.standard.object(forKey: "highscore")
        
        if storedHighScore == nil {
            highScore = 0
            highScoreLabel.text = "Highscore: \(highScore)"
        }
        
        if let newScore = storedHighScore as? Int {
            highScore = newScore
            highScoreLabel.text = "Highscore: \(highScore)"
        }
        
        image1.isUserInteractionEnabled = true
        image2.isUserInteractionEnabled = true
        image3.isUserInteractionEnabled = true
        image4.isUserInteractionEnabled = true
        image5.isUserInteractionEnabled = true
        image6.isUserInteractionEnabled = true
        image7.isUserInteractionEnabled = true
        image8.isUserInteractionEnabled = true
        image9.isUserInteractionEnabled = true
        
        let recognize1 = UITapGestureRecognizer(target: self, action: #selector(ViewController.increaseScore))
        let recognize2 = UITapGestureRecognizer(target: self, action: #selector(ViewController.increaseScore))
        let recognize3 = UITapGestureRecognizer(target: self, action: #selector(ViewController.increaseScore))
        let recognize4 = UITapGestureRecognizer(target: self, action: #selector(ViewController.increaseScore))
        let recognize5 = UITapGestureRecognizer(target: self, action: #selector(ViewController.increaseScore))
        let recognize6 = UITapGestureRecognizer(target: self, action: #selector(ViewController.increaseScore))
        let recognize7 = UITapGestureRecognizer(target: self, action: #selector(ViewController.increaseScore))
        let recognize8 = UITapGestureRecognizer(target: self, action: #selector(ViewController.increaseScore))
        let recognize9 = UITapGestureRecognizer(target: self, action: #selector(ViewController.increaseScore))
        
        imageArray = [image1, image2, image3, image4, image5, image6, image7, image8, image9]
        
        //imageArray.append(image1)
        image1.addGestureRecognizer(recognize1)
        image2.addGestureRecognizer(recognize2)
        image3.addGestureRecognizer(recognize3)
        image4.addGestureRecognizer(recognize4)
        image5.addGestureRecognizer(recognize5)
        image6.addGestureRecognizer(recognize6)
        image7.addGestureRecognizer(recognize7)
        image8.addGestureRecognizer(recognize8)
        image9.addGestureRecognizer(recognize9)
       
        //TIMER
        counter = 30
        timeLabel.text = String(counter)
        
        time = Timer.scheduledTimer(timeInterval: 1, target: self, selector: #selector(countBack), userInfo: nil, repeats: true)
        hideTimer =  Timer.scheduledTimer(timeInterval: 0.5, target: self, selector: #selector(hideKendy), userInfo: nil, repeats: true)
    
        //CALL FUNCTION
        hideKendy()
        
    }
    
    @objc func hideKendy() { // Hide the images when it clicked
        
        for kendy in imageArray {
            kendy.isHidden = true
        }
        
        // Rastgele sayı oluşturmak
        let random = Int(arc4random_uniform(UInt32(imageArray.count - 1)))
        imageArray[random].isHidden = false
    }
    
    @objc func increaseScore() { // When we click to image
        
        score = score + 1
        scoreLabel.text = "Score: \(score)"
    }
    
    @objc func countBack() { // Need to count the time
        
        counter = counter - 1
        timeLabel.text = String(counter)
        
        if counter == 0 {
            time.invalidate() // Time was stoped
            hideTimer.invalidate()
            
            for kendy in imageArray {
                kendy.isHidden = true
            }
            
            // High Score
            if self.score > self.highScore {
                self.highScore = self.score
                highScoreLabel.text = "Highscore: \(self.highScore)"
                UserDefaults.standard.set(self.highScore, forKey: "highscore")
            }
            
            // Alert
            let alert = UIAlertController(title: "Time is Over", message: "Do you want to play again?", preferredStyle: UIAlertController.Style.alert)
            
            let ok = UIAlertAction(title: "OK", style: UIAlertAction.Style.cancel, handler: nil)
            
            let replay = UIAlertAction(title: "Replay", style: UIAlertAction.Style.default) { (UIAlertAction) in
                
                // REPLAY ACTION
                self.score = 0
                self.scoreLabel.text = "Score: \(self.score)"
                self.counter = 30
                self.timeLabel.text = String(self.counter)
                
                self.time = Timer.scheduledTimer(timeInterval: 1, target: self, selector: #selector(self.countBack), userInfo: nil, repeats: true)
                self.hideTimer =  Timer.scheduledTimer(timeInterval: 0.5, target: self, selector: #selector(self.hideKendy), userInfo: nil, repeats: true)
                
            }
            
            alert.addAction(ok)
            alert.addAction(replay)
            self.present(alert, animated: true, completion: nil)
            
        }
    }
}
