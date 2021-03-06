# Stanford-CS193p-Spring-2021
A programming course using Swift and SwiftUI that is offered by Stanford University online for free.

Stanford's CS193p course, Developing Applications for iOS, explains the fundamentals of how to build applications for iPhone and iPad using SwiftUI. Most recently offered in Spring quarter 2021, the lectures were given to Stanford students in an on-line format due to the novel coronavirus pandemic and are now being made available (at least 2 per week) to all via Stanford's YouTube channel.

The course was also offered (and videos made available) in 2020. That version remains archived here, but if you are new to CS193p content, you'll definitely only need to watch the 2021 videos. The only exceptions are Lectures 11 & 12 of 2020 (Enroute) which contains material not covered in 2021.

On this site, you will be able to find materials that were distributed to students during the quarter such as homework assignment write-ups and demo code. Unfortunately, we cannot offer any of the same kind of direct support we gave our students (on-line Q&A and office hours with teaching staff, homework grading, etc.), but the materials posted here should still be helpful in understanding the lectures as you watch. As we emphasize to our students, doing the homework assignments is absolutely essential to learning the material in this course.

SwiftUI is fairly new, having shipped just over a year before this course was taught. Thus it may well be that by the time you are watching it, some of the course's content will already be out of date as updates to SwiftUI occur, requiring some adjustment as you watch. That is normal for new technology.

The material in this course was not developed with the involvement of, nor was it vetted by, anyone at Apple, so it should not be perceived as "the truth" for how to develop using SwiftUI. We've done our best to understand this technology ourselves in the short time it has been out and then share what we've learned. Enjoy!

### *Note: Look at commit history to see code from various assignments/ lectures*

## Assignment I

### Task 1: 
Get the Memorize game working as demonstrated in lectures 1 and 2. Type in all the code. Do not copy/paste from anywhere.

### Task 2: 
You can remove the ??? and ??? buttons at the bottom of the screen.


### Task 3:
Add a title ???Memorize!??? to the top of the screen.

```swift
Text("Memorize!").font(.largeTitle)
```

### Task 4 - 9: 
Add at least 3 ???theme choosing??? buttons to your UI, each of which causes all of the cards to be replaced with new cards that contain emoji that match the chosen theme. You can use Vehicles from lecture as one of the 3 themes if you want to, but you are welcome to create 3 (or more) completely new themes.

The number of cards in each of your 3 themes should be different, but in no case fewer than 8.

The cards that appear when a theme button is touched should be in an unpredictable (i.e. random) order. In other words, the cards should be shuffled each time a theme button is chosen.

The theme-choosing buttons must include an image representing the theme and text describing the theme stacked on top of each other vertically.

The image portion of each of the theme-choosing buttons must be created using an SF Symbol which evokes the idea of the theme it chooses (like the car symbol and the Vehicles theme shown in the Screenshot section below).

The text description of the theme-choosing buttons must use a noticeably smaller font than the font we chose for the emoji on the cards.

```swift
var vehicles: some View {
        Button {
            emojis = ["????", "??????", "????", "????", "????", "????", "????", "????", "????", "????", "????", "????"].shuffled()
            randomNumberEmojis = Int.random(in: 4..<emojis.count)
        } label: {
            VStack {
                Image(systemName: "car")
                Text("Vehicles")
                    .font(.footnote)
            }
        } .padding(.horizontal)
    }
    
    var bugs: some View {
        Button {
            emojis = ["????", "????", "????", "????", "????", "????", "????", "????", "????", "????"].shuffled()
            randomNumberEmojis = Int.random(in: 4..<emojis.count)
        } label: {
            VStack {
                Image(systemName: "ant")
                Text("Bugs")
                    .font(.footnote)
            }
        } .padding(.horizontal)
    }
    
    var faces: some View {
        Button {
            emojis = ["?????????????", "???????????", "????????", "???????????????", "???????????????", "????", "?????????????????", "????????", "????????", "????????", "????????", "?????????????????", "????????", "????????"].shuffled()
            randomNumberEmojis = Int.random(in: 4..<emojis.count)
        } label: {
            VStack {
                Image(systemName: "face.smiling")
                Text("Faces")
                    .font(.footnote)
            }
        } .padding(.horizontal)
    }
```

```swift
HStack(alignment: .bottom) {
                vehicles
                Spacer()
                bugs //add theme buttons here
                Spacer()
                faces
            }
            .font(.largeTitle)
            .padding(.horizontal)
```

### Task 10: 
Your UI should work in portrait or landscape on any iPhone. This probably will not require any work on your part (that???s part of the power of SwiftUI), but be sure to experiment with running on different simulators in Xcode to be sure.

## Assignment II

### Task 1: 
Get the Memorize game working as demonstrated in lectures 1 through 4. Type in all the code. Do not copy/paste from anywhere.

### Task 2: 
If you???re starting with your assignment 1 code, remove your theme-choosing buttons and (optionally) the title of your game.

### Task 3: 
Add the formal concept of a ???Theme??? to your Model. A Theme consists of a name for the theme, a set of emoji to use, a number of pairs of cards to show, and an appropriate color to use to draw the cards.

```swift
struct Theme<Content> {
    let themeName: String
    var setOfEmojiForTheme: [Content]
    var numberOfPairs: Int?
    var themeColor: String  //made themeColor a String, as this file is technically part of the model, which is supposed to be UI independent.
    var useGradient: Bool
    
    ...
    
}
```

### Task 4: 
At least one Theme in your game should show fewer pairs of cards than the number of emoji available in that theme.

```swift
Theme(themeName: "Barn Animals", setOfEmojiForTheme: ["????", "????", "????", "????", "????", "????", "????", "????"], numberOfPairs: 5, themeColor: "yellow")
```

### Task 5: 
If the number of pairs of emoji to show in a Theme is fewer than the number of emojis that are available in that theme, then it should not just always use the first few emoji in the theme. It must use any of the emoji in the theme. In other words, do not have any ???dead emoji??? in your code that can never appear in a game.

```swift
static func createMemoryGame(with theme: Theme<String>) -> MemoryGame<String> {
        let shuffledSetOfEmojis = theme.setOfEmojiForTheme.shuffled()
        
        return MemoryGame<String>(numberOfPairsOfCards: theme.numberOfPairs!) { pairIndex in  //added a force unwrap of numberOfPairs
            //theme.setOfEmojiForTheme[pairIndex]
            shuffledSetOfEmojis[pairIndex]
        }
    }
```
### Task 6 - 7: 
Never allow more than one pair of cards in a game to have the same emoji on it.

If a Theme mistakenly specifies to show more pairs of cards than there are emoji available, then automatically reduce the count of cards to show to match the count of available emoji.

```swift
    //init created within Theme.swift
    init(themeName: String, setOfEmojiForTheme: [Content], numberOfPairs: Int?, themeColor: String, useGradient: Bool = false) {
        self.themeName = themeName
        self.setOfEmojiForTheme = setOfEmojiForTheme
        let contentCount = setOfEmojiForTheme.count
        self.numberOfPairs = numberOfPairs ?? contentCount //use nil-coalescing operator, as value initiliazed can still be nil. This makes a force unwrap in the viewModel and next line safe.
        if(self.numberOfPairs! > contentCount) {
            self.numberOfPairs = contentCount
        }
        
        //Alternatively: (Not quite as readable in my opinion)
        //self.numberOfPairs = (self.numberOfPairs! < setOfEmojiForTheme.count) ? self.numberOfPairs : setOfEmojiForTheme.count
        self.themeColor = themeColor
        self.useGradient = useGradient
    }
```

### Task 8 - 9: 
Support at least 6 different themes in your game.

A new theme should be able to be added to your game with a single line of code.

```swift
//viewModel (EmojiMemoryGame.swift)
static var themes: [Theme<String>] = [
        Theme(themeName: "Vehicles", setOfEmojiForTheme: ["????", "??????", "????", "????", "????", "????", "????", "????", "????", "????", "????", "????"], numberOfPairs: 6, themeColor: "red"),
        Theme(themeName: "Barn Animals", setOfEmojiForTheme: ["????", "????", "????", "????", "????", "????", "????", "????"], numberOfPairs: 5, themeColor: "yellow"),
        Theme(themeName: "Faces", setOfEmojiForTheme: ["?????????????", "???????????", "????????", "???????????????", "???????????????", "????", "?????????????????", "????????", "????????", "????????", "????????", "?????????????????", "????????", "????????"], numberOfPairs: Int.random(in: 5..<8), themeColor: "blue", useGradient: true),
        Theme(themeName: "Bugs", setOfEmojiForTheme: ["????", "????", "????", "????", "????", "????", "????", "????", "????", "????"], numberOfPairs: 8, themeColor: "green"),
        Theme(themeName: "Flags", setOfEmojiForTheme: ["????????", "????????", "????????????????????????????", "????????", "????????", "????????", "????????"], numberOfPairs: 8, themeColor: "purple", useGradient: true),
        Theme(themeName: "Halloween", setOfEmojiForTheme: ["????", "????", "????", "????", "????"], themeColor: "orange")
    ]
```

### Task 10 - 13: 
Add a ???New Game??? button to your UI (anywhere you think is best) which begins a brand new game.

A new game should use a randomly chosen theme and touching the New Game button should repeatedly keep choosing a new random theme.

The cards in a new game should all start face down.

The cards in a new game should be fully shuffled. This means that they are not in any predictable order, that they are selected from any of the emojis in the theme (i.e. Required Task 5), and also that the matching pairs are not all side-by-side like they were in lecture (though they can accidentally still appear side-by-side at random).

```swift
//View
Button(action: {viewModel.resetGame()}, label: {
                Text("New Game")
            }).padding()
```
```swift
//ViewModel
func resetGame() {
        currentTheme = EmojiMemoryGame.themes.randomElement()!
        model = EmojiMemoryGame.createMemoryGame(with: currentTheme)
    }
```

### Task 14: 
Show the theme???s name in your UI. You can do this in whatever way you think looks best.

```swift
//View
Text("\(viewModel.themeName)").font(.largeTitle)
```
```swift
//ViewModel
var themeName: String {
        currentTheme.themeName
    }
```
### Task 15: 
Keep score in your game by penalizing 1 point for every previously seen card that is involved in a mismatch and giving 2 points for every match (whether or not the cards involved have been ???previously seen???). See Hints below for a more detailed explanation. The score is allowed to be negative if the user is bad at Memorize.

```swift
//Added hasBeenSeen to Card struct: 

struct Card: Identifiable {
        var isFaceUp: Bool = false
        var isMatched: Bool = false
        var hasBeenSeen: Bool = false
        var content: CardContent
        var id: Int
    }
```
```swift
//Updated scoreOfTheGame throughout choose func
private var indexOfTheOneAndOnlyFaceUpCard: Int?
private(set) var scoreOfTheGame: Int = 0  //Initialize to nil. In view, if nil score is --, otherwise, score is an integer.
    
    mutating func choose(_ card: Card) {
        if let chosenIndex = cards.firstIndex(where: {$0.id == card.id}),
           !cards[chosenIndex].isFaceUp,
           !cards[chosenIndex].isMatched
        {
            
            if let potentialMatchIndex = indexOfTheOneAndOnlyFaceUpCard {
                if cards[chosenIndex].content == cards[potentialMatchIndex].content {
                    cards[chosenIndex].isMatched = true
                    cards[potentialMatchIndex].isMatched = true
                    scoreOfTheGame += 2
                } else {
                    if cards[potentialMatchIndex].hasBeenSeen == true {
                        scoreOfTheGame -= 1
                    }
                    if cards[chosenIndex].hasBeenSeen == true {
                        scoreOfTheGame -= 1
                    } //subtracts 1 if the second card selected has been seen when mismatached
                    cards[chosenIndex].hasBeenSeen = true
                    cards[potentialMatchIndex].hasBeenSeen = true
                }
                indexOfTheOneAndOnlyFaceUpCard = nil
                
            } else {
                for index in cards.indices {
                    cards[index].isFaceUp = false
                } //turn cards face down, as getting to else means indexOfTheOneAndOnlyFaceUpCard is nil (none are up, or more than one)
                indexOfTheOneAndOnlyFaceUpCard = chosenIndex
            }

//            cards[chosenIndex].hasBeenSeen = true
            cards[chosenIndex].isFaceUp.toggle()
        }
        //print("\(cards)")
    }
```

### Task 16:
Display the score in your UI. You can do this in whatever way you think looks best.

```swift
//View
Text("Score: \(viewModel.scoreOfTheGame)").font(.title).padding()
```
```swift
//ViewModel
var scoreOfTheGame: Int {
        model.scoreOfTheGame
    }
```

### Extra Credit 1: 
When your code creates a Theme, allow it to default to use all the emoji available in the theme if the code that creates the Theme doesn???t want to explicitly specify how many pairs to use. This will require adding an init or two to your Theme struct.

```swift
init(themeName: String, setOfEmojiForTheme: [Content], themeColor: String, useGradient: Bool = false) {
        self.themeName = themeName
        self.setOfEmojiForTheme = setOfEmojiForTheme
        self.numberOfPairs = setOfEmojiForTheme.count   //Because it is not declared, this defaults to nil
        self.themeColor = themeColor
        self.useGradient = useGradient
    }
```

### Extra Credit 2:
Allow the creation of some Themes where the number of pairs of cards to show is not a specific number but is, instead, a random number. We???re not saying that every Theme now shows a random number of cards, just that some Themes can now be created to show a random number of cards (while others still are created to show a specific, pre-determined number of cards).

```swift
Theme(themeName: "Faces", setOfEmojiForTheme: ["?????????????", "???????????", "????????", "???????????????", "???????????????", "????", "?????????????????", "????????", "????????", "????????", "????????", "?????????????????", "????????", "????????"], numberOfPairs: Int.random(in: 5..<8), themeColor: "blue", useGradient: true)
```

### Extra Credit 3: 
Support a gradient as the ???color??? for a theme. Hint: fill() can take a Gradient as its argument rather than a Color. This is a ???learning to look things up in the documentation??? exercise.

```swift
//Added useGradient var to Theme struct
var useGradient: Bool
```

```swift
//Within the CardView struct (View) 
if useGradient {
        shape.fill(LinearGradient(gradient: Gradient(colors: [themeColor, Color.pink]), startPoint: .top, endPoint: .bottom))
} else {
        shape.fill(themeColor)
}
```

