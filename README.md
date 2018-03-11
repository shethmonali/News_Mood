

```python
# Dependencies
import tweepy
import json
import numpy as np
import matplotlib.pyplot as plt
import pandas as pd
import seaborn as sns
from vaderSentiment.vaderSentiment import SentimentIntensityAnalyzer
analyzer = SentimentIntensityAnalyzer()
import time
```


```python
# Twitter API Keys
from config import (consumer_key, 
                    consumer_secret, 
                    access_token, 
                    access_token_secret)

# Setup Tweepy API Authentication
auth = tweepy.OAuthHandler(consumer_key, consumer_secret)
auth.set_access_token(access_token, access_token_secret)
api = tweepy.API(auth, parser=tweepy.parsers.JSONParser())
```


```python
# Target Search Term
news_outlet = ["@BBC", "@CBS", "@CNN", "@Fox", "@nytimes"]
counter = 1
sentiments =[]
```


```python
for outlet in news_outlet:
    public_tweets = api.user_timeline(outlet, count=100)
    tweetnumber = 1       
    for tweet in public_tweets:
        print("Tweet %s: %s" % (counter, tweet["text"]))
        compound = analyzer.polarity_scores(tweet["text"])["compound"]
        pos = analyzer.polarity_scores(tweet["text"])["pos"]
        neu = analyzer.polarity_scores(tweet["text"])["neu"]
        neg = analyzer.polarity_scores(tweet["text"])["neg"]
        tweets_ago = tweetnumber
        sentiments.append({"Media Source": outlet,
                           "Text":tweet["text"],
                           "Date": tweet["created_at"],
                           "Compound": compound,
                           "Positive": pos,
                           "Neutral": neu,
                           "Negative": neg,
                           "Tweet Count": tweetnumber})
        tweetnumber +=1
        counter +=1
```

    Tweet 1: 🐑 Ewe won't believe how this farmer revives frozen little lambs.  https://t.co/z1UvoIiRXY
    Tweet 2: RT @BBCMOTD: John Motson in his element during his last ever LIVE commentary (would you believe it?!)
    50 years as a commentator🎙
    10 World C…
    Tweet 3: By 2050, 30-50% of all species could be heading towards extinction. https://t.co/2TYrhIiBIw
    Tweet 4: 🗑🚀 Tomorrow’s space scientists will have to deal with a monumental amount of space junk left from previous spacecra… https://t.co/IJTpc5TBsd
    Tweet 5: RT @5liveSport: For the final time...
    
    The legendary John Motson is in our commentary box 🎙️⚽️
    
    He'll be commentating on #Arsenal vs #Watfo…
    Tweet 6: Making fake videos just got easier. https://t.co/VSP9bwHVNZ
    Tweet 7: Captain America teams up with Black Widow to get to the bottom of a conspiracy. 🔥💪🇺🇸
    
    Captain America: The Winter S… https://t.co/yDWYJRUgGi
    Tweet 8: 🚐 Meet Om Abdullah - Egypt's first official female minibus driver. https://t.co/Z4fmE1k5ph
    Tweet 9: RT @BBCSport: We're not emotionally ready for this.
    
    John Motson will deliver his final live BBC commentary today.
    
    More details👇
    https://t…
    Tweet 10: RT @CBeebiesHQ: Doesn't matter if your family has:
    
    One Mum👩‍👦
    
    Two Mums 👩‍👩‍👧‍👧
    
    A Grandmum 👵🏼
    
    ... they all want a lie-in!
    
    #HappyMothers…
    Tweet 11: 🍰 Surprise your mum with a homemade treat this #MothersDay – from easy cupcakes to luscious layer cakes.
    👉… https://t.co/p7n5lo2ziD
    Tweet 12: RT @BBCScotland: Undercover filming reveals Scotland's growing 'bag' problem.
    
    See more from #ShortStuff -  https://t.co/orhxUmskEd 
    
    #come…
    Tweet 13: 🐷 Blossom the pig can sit on command! https://t.co/mRGtuk0zqh
    Tweet 14: RT @BBC6Music: 🎂 6 Music launched 16 years ago today. 
    
    When you turned 16, which album did you think was a masterpiece? Do you still love…
    Tweet 15: RT @1Xtra: The new breed a dancehall artist @thebellablair performs for @davidrodigan #1XtraJamaica 
    
    📺▶ https://t.co/MrnHDz2ZyL https://t.…
    Tweet 16: RT @BBCRadio3: "To my first Love, my Mother, on whose knee
    I learnt love-lore that is not troublesome"
    Christina Rossetti's 'Sonnets are fu…
    Tweet 17: RT @TWBBC: Fry-up anyone?  🌭🥓🍳 Did you know just 1.5 sausages could reduce your lifespan by 30 minutes? But 2-3 cups of coffee could gain y…
    Tweet 18: During the Middle Ages the custom developed of allowing people who had moved away to return and visit their mothers… https://t.co/UQD2ySeRH4
    Tweet 19: 🎤🎶 Singing is for everyone... even if you can't sing. https://t.co/eVIdGhswUT
    Tweet 20: ❤️ Happy #MothersDay to all of the amazing mums out there, the mums-to-be and the mums who live on in our hearts.… https://t.co/v9yrzvfhqO
    Tweet 21: RT @BBCWales: 💐 What better way to say Happy Mother's Day than with a marvellous marshmallow bouquet! 💐
    
    🍡 Let little hands tackle this tas…
    Tweet 22: RT @BBCTwo: Celebrate #MothersDay with the #Motherland guide to good parenting... 😂 https://t.co/GMZLEfWuaF
    Tweet 23: RT @bbcasiannetwork: 🎇The Desi party don't start until you've heard @jennyjohalmusic singing 'Narma'  🎇 
    #AsianNetworkLive https://t.co/lah…
    Tweet 24: RT @BBCRadio2: She did it! Over 300 miles cycled. Over £500,000 raised so far for @SportRelief.
    
    @ZoeTheBall, we are so proud! 👏
    
    Donate to…
    Tweet 25: RT @BBCTwo: A shocking moment in history that changed the fashion world forever.
    
    #ACSVersace continues Wednesday, 9pm, @BBCTwo. https://t.…
    Tweet 26: RT @BBCWales: Meet YouTube superstar @Gonth93
    
    💰 Welcome to the world of #YoungWelshPrettyMinted 💰
    
    Tonight, 10.35pm - @BBCOne Wales
    Availa…
    Tweet 27: RT @bbcrb: This. Is. So. Cute! 😍
    Who wouldn't love a tiny meal like this! 🤤🥘 https://t.co/73t4rhbopo
    Tweet 28: RT @BBCScotland: Cathy's been having a great time at the spa! 🇧🇷
    
    Watch #TwoDoorsDown on @BBCiPlayer - https://t.co/JgbkHFXufb https://t.co…
    Tweet 29: RT @bbcasiannetwork: 🎤 Don't miss a single bit of #AsianNetworkLive🎶 
    Here's how to make sure you catch ALL the action 👉 https://t.co/AZIif…
    Tweet 30: RT @BBCiPlayer: Police officer Kris was left paralysed after being hit by a van in the Westminster bridge terrorist attack. Seven months la…
    Tweet 31: RT @BBCSport: Ireland have been crowned Six Nations champions with a match to spare.
    
    https://t.co/YFyOxSY0Tp #SixNations https://t.co/K34r…
    Tweet 32: RT @bbcasiannetwork: 💥#AsianNetworkLive is taking over the airwaves tonight!
    Catch all the action from
    🔴 6pm on @bbcasiannetwork
    🔴 7pm on @…
    Tweet 33: RT @bbcasiannetwork: Which #AsianNetworkLive performance can't you wait for? 🤩 
    🎤 @jaysean
    🎤 @JasmineSandlas
    🎤 @lottoboyzz_
    🎤 @omgAdamSaleh…
    Tweet 34: 🐮🥛 Glass from the past. https://t.co/stRtHiAGGI
    Tweet 35: 📸❤️ A different portrait of black fatherhood. 
    👉 https://t.co/SX1Lbqsss0 https://t.co/WQnNbAfIYK
    Tweet 36: Have we got depression wrong? https://t.co/yFPp67xmxe
    Tweet 37: RT @mixital: Enjoying @BBCOne’s Troy? 
    
    Join our new creative challenge &amp; create your own #Troy art! Grab images from our website, make the…
    Tweet 38: RT @BBCWales: 'Feel the fear and do it anyway'
    
    That's Dilys's motto, and she's the @GWR holder for the Oldest Woman Skydiver in the world!…
    Tweet 39: 🤖😱 The robots are coming! 
    
    And this time they want to design and assemble your furniture. 😄 https://t.co/naCu7LJ0Mf
    Tweet 40: ❤️🐿 The pine marten has emerged as an unlikely ally for the beleaguered native red squirrel in its battle with the… https://t.co/aaJ1XuLJ78
    Tweet 41: 🍴 Do you know how many calories were in your lunch?https://t.co/ouomQAA92I
    Tweet 42: Hindered by short-term memory loss, a man struggles to piece his life back together after his wife's murder.… https://t.co/IDASykaROO
    Tweet 43: Heartbreaking. 
    https://t.co/W8GJNKq8rm
    Tweet 44: 😂🔥 @GordonRamsay's daughter got him good! https://t.co/0dqwntX5uj
    Tweet 45: ❤️😭 We're not crying... you're crying. 
    #TheSilentChild #Oscars https://t.co/ienD3dDtKX
    Tweet 46: 😱 Baptism at this church can be a matter of life and death. https://t.co/aEsVqMUPQ2
    Tweet 47: Don't look down! 😣 https://t.co/r4ChPajO4A
    Tweet 48: RT @BBCRadioWales: "The pain is so severe it feels like my body wants to explode." 
    
    #Endometriosis is as common as asthma but it takes, on…
    Tweet 49: RT @BBCSpringwatch: We LOVE hedgehogs, but over the last decade we have lost over 40% of our prickly neighbours 😞 Billy's here to give us a…
    Tweet 50: RT @bbcasiannetwork: 💃🕺 #AsianNetworkLive is TONIGHT! We are so excited!💃🕺 
    📻 Broadcasting live from ✨6pm✨ on Asian Network and 
    ✨7pm✨ on @…
    Tweet 51: RT @BBCAfrica: Could Nigeria's animation houses become as big as Nollywood? #Nigeria #Animation #Yoruba #Lagos #cartoons https://t.co/3Htr9…
    Tweet 52: RT @bbcthree: This woman is leading the #disabilibabe movement https://t.co/FlgCordJaC
    Tweet 53: RT @BBCArchive: #OnThisDay 1977: Still searching for the perfect Mother's Day present? We humbly suggest a homemade thermodynamicizer - jus…
    Tweet 54: RT @bbcthesocial: Greg is 23 years old and a world piping champion. #IntoIt
    
    Happy International Bagpipe Day! https://t.co/74f3USdg3H
    Tweet 55: From boar hair toothbrushes to beeswax food wrap, here's how you can dump the disposable plastics ✅… https://t.co/ydpoHWlJxA
    Tweet 56: How is plastic affecting seabirds? 
    #OurBluePlanet #BluePlanet2 https://t.co/bcvGKZjsJF
    Tweet 57: Do you have a secret British accent? 🤔Take this quiz and we’ll pinpoint which part of the UK you most sound like yo… https://t.co/fiZDg5pK85
    Tweet 58: 🙌 1 year ago today the world became a better place.
    #BBCdad https://t.co/Q5I70XxT7T
    Tweet 59: A new gardening year is beginning at Longmeadow. 🍃🌸🌱🌷
    
    Gardener's World | 8:30pm | @BBCTwo | https://t.co/R2ZqqlaEbQ https://t.co/A9Q43XOoxh
    Tweet 60: 💘 @MabelMcVey covering @Coldplay's 'Fix You' in the Live Lounge is just perfect. 🙌 https://t.co/Sk9yMymZZs
    Tweet 61: RT @bbcthesocial: Instead of a plastic straw, why not use a condom? 😏 https://t.co/XBtyZHdfkb
    Tweet 62: Want to improve your persuasion powers? 
    
    😻 Just watch a cat. https://t.co/vA9asFlD9g
    Tweet 63: RT @bbcworldservice: These images of women on their period are greeting commuters in Stockholm. For more listen/download 📻: https://t.co/FL…
    Tweet 64: RT @bbcwritersroom: "I felt like a real writer by the end of it!" 
    
    Find out how we created 6 #Comedy monologues for @bbcasiannetwork #Asia…
    Tweet 65: RT @BBCScotland: "If there is a real live superwoman, she is it."
    
    Meet the surgeon who walked for eight miles in the heavy snow so she cou…
    Tweet 66: RT @BBCFOUR: Are we really born to prefer 'boys' or 'girls' toys? The answer might lie with monkeys... https://t.co/TPHeE7cStK
    Tweet 67: Noel and Liam Gallagher will headline @BBCRadio2's stages at the BBC's Biggest Weekend Festival in May 🎤👌… https://t.co/q7YblDKT2N
    Tweet 68: ♻️ The packaging industry in England has been accused of exaggerating the amount of its plastic that gets recycled.… https://t.co/lHpw5rVHub
    Tweet 69: Over 80 but as healthy as a 20-year-old: How exercise in old age prevents the immune system from declining… https://t.co/jABPPvkafN
    Tweet 70: It took almost a year to get to the final look of Snoke. 
    #StarWars #TheLastJedi https://t.co/o6ttTvHdQy
    Tweet 71: RT @BBCWorld: Winter Paralympics opening ceremony begins in Pyeongchang https://t.co/VP4W2q1Hhe
    Tweet 72: RT @bbcthree: BBC Three presents Perfect Day #perfectday2018 https://t.co/oFgg0vJJgP
    Tweet 73: RT @BBCR1: Happy Birthday, Suga @BTS_twt! 🐻🎈✨ https://t.co/7Al640sBgB
    Tweet 74: RT @bbcarts: 🎶 This is the amazing moment when eight London teenagers hear an #opera singer in the flesh for the first time. Watch the full…
    Tweet 75: "For the first time, we think we’re actually able to recreate real living heart tissue which is identical to that o… https://t.co/kGIMWWBf4z
    Tweet 76: The #BiggestWeekend lineup just got bigger! More artists have been added to all four locations including… https://t.co/5G2GqBHkHK
    Tweet 77: 🐑 This Lancashire farmer revives frozen lambs by sticking them inside an oven! https://t.co/z1UvoIiRXY
    Tweet 78: 🌊📝 The world's oldest known message in a bottle has been found, almost 132 years after it was thrown into the sea.… https://t.co/fHdTDbK2Mg
    Tweet 79: 🍋☕️ Fruit tea is good for you. Right...? https://t.co/YExPNJG6bE
    Tweet 80: This Ancient Egyptian pharaoh invested more in images of himself than any other ruler, and for him, size really did… https://t.co/IUlHKvTATa
    Tweet 81: Jack Jarvis and Victor McDade are back for another hilarious six-part series. 
    
    #StillGame | 9:30pm | @BBCOne |… https://t.co/zFtd4VQyKL
    Tweet 82: Lucy and Lee are back! 🙌🎉
    
    #NotGoingOut | 9pm | @BBCOne | https://t.co/dUnnFaIoci https://t.co/gSpTMCPrSC
    Tweet 83: What advice would YOU give your younger self? 🤔
    
    #InternationalWomensDay #IWD2018 https://t.co/vIwPLtzTbO
    Tweet 84: In recognition of International Women’s Day, a playlist in celebration of the funniest, saddest and most significan… https://t.co/4WCrxvJxFY
    Tweet 85: 🔬 More than half of the UK can't name a famous woman in science. 
    
    Time to change that. 
    👉 https://t.co/3HVhvFaxaT… https://t.co/KL0D1vIw8S
    Tweet 86: ✍️🙌 Lotte Reiniger is the unsung heroine of early animation. 
    
    #InternationalWomensDay #IWD2018 https://t.co/Z4qDFWXcCS
    Tweet 87: RT @BBCMOTD: Favourite emoji?
    Champions League or World Cup?
    
    @TekkerzKid quizzes @ManUtd striker @MarcusRashford for #BBCOwnIt.
    
    📺 Check o…
    Tweet 88: ⚽️🥅 The story of the fall and rise of women's football, told through 10 objects collected by the National… https://t.co/xOi8wNW64N
    Tweet 89: RT @bbcmusic: 🙌 @Camila_Cabello just dropped the video for 'Never Be The Same' 💕
    What do you think?
    
    Watch 👉 https://t.co/gv60m6x9Wk https:…
    Tweet 90: Nine-year-old Lola thinks more girls should take up skateboarding.
    
    Lola is rad. 🤘😝
    
    #InternationalWomensDay… https://t.co/EeOygOVfoh
    Tweet 91: 🎨🖼 #AnnieSwynnerton: The woman who forced open the male art world and blazed a trail for female artists.
    👉… https://t.co/rHDoqWL4iI
    Tweet 92: 🎓🎒 These women are pushing young girls into school, not marriage. 
    
    #InternationalWomensDay #IWD2018 https://t.co/TIf5lbu3GZ
    Tweet 93: RT @bbcworldservice: In Senegal, graffiti artist Zeinixx is campaigning for women's rights... with spray paint. 🇸🇳 https://t.co/cy2ddCmEKo
    Tweet 94: RT @bbc5live: “That was the end of my childhood”
    
    Money saving expert @MartinSLewis opens up to @tonylivesey about the death of his mum, th…
    Tweet 95: 🎤 🎧Grime through time: A brief history of one of the biggest music genres to come out of the UK https://t.co/876CsOCFsa
    Tweet 96: RT @BBCRadio4: Ten female authors who changed sci-fi: https://t.co/r5s1mqYrQD
    #IWD2018 https://t.co/cQxHsT9c3E
    Tweet 97: Thousands of women in the UK can't afford to buy tampons and towels. 
    
    #InternationalWomensDay #IWD2018 https://t.co/66P7IrZ9lB
    Tweet 98: 👷👷‍♀️ Building future engineers - it's a lot easier than you think! 
    
    #BitzAndBob | @CbeebiesHQ https://t.co/YV3kTIj0Dx
    Tweet 99: RT @BBCR1: 💛💛💛 
    
    Who's ready for @thegreatkhalid in the Live Lounge?
    
    Make sure you're listening to @claraamfo at 12pm 👉 https://t.co/48Ruy…
    Tweet 100: Life advice from one of our all-time favourite women, @DawnFrenchUK.  
    
    #InternationalWomensDay #IWD2018 https://t.co/MAOUgpWb7J
    Tweet 101: RT @DierksBentley: Take and post a photo of the woman in your life who inspires you daily! Use the hashtag #WomanAmenACM in your post for a…
    Tweet 102: RT @MomCBS: If you missed guest star @KChenoweth in the latest episode of #Mom, not to worry! Watch now: https://t.co/RlvXoGOZ0l https://t.…
    Tweet 103: Give a round of applause to @KelseaBallerini, @MirandaLambert, @Reba, @MarenMorris, and @CarrieUnderwood, the five… https://t.co/Ncp1BTXx6N
    Tweet 104: RT @thegoodfight: Smart, sexy, and sophisticated. See what's coming this season on #TheGoodFight. https://t.co/CuKhx2G50P https://t.co/ygTI…
    Tweet 105: RT @BlueBloods_CBS: Even stand-up guys fall down sometimes. #BlueBloods is new tonight at 10/9c! https://t.co/UOlDm22wWW
    Tweet 106: Today and every day we celebrate the women in our lives who empower and inspire us. Share a story about an influent… https://t.co/9rVtqrElvT
    Tweet 107: Take and post a photo of the woman in your life who inspires you daily! Use the hashtag #WomanAmenACM in your post… https://t.co/7ShhvE48zy
    Tweet 108: RT @thegoodfight: Meticulously constructed. Soapy &amp; sexy. Intoxicating, savage television. 🔥 Here's what critics are saying about #TheGoodF…
    Tweet 109: This just in! @Jason_Aldean, @mirandalambert, @LukeBryanOnline, and many more are set to perform at the 53rd Academ… https://t.co/mfxw2VxzU4
    Tweet 110: Meet the ensemble of talented actors slated to join $1, a new mystery series coming to CBS All Access:… https://t.co/QoyYv7vxwg
    Tweet 111: Will @Jason_Aldean, @garthbrooks, @LukeBryanOnline, @ChrisStapleton, or @KeithUrban be named Entertainer Of The Yea… https://t.co/rMD8zjeX3s
    Tweet 112: RT @thegoodfight: It feels good to be back. 👠💄🔥 The season 2 premiere of #TheGoodFight is now streaming, exclusively on CBS All Access: htt…
    Tweet 113: RT @thegoodfight: Tomorrow, #TheGoodFight is back. Stream the season 2 premiere only on CBS All Access: https://t.co/tNFR8LBJO2 https://t.c…
    Tweet 114: Who are the trailblazing women in your life that inspire you? Join CBS and the ANA's #SeeHer initiative, celebratin… https://t.co/M0KqZ41Bes
    Tweet 115: Join @maria_bello, @aishatyler and @TeaLeoni in celebrating the accomplishments of women who have contributed to th… https://t.co/MefESBeFL3
    Tweet 116: In honor of Women's History Month, CBS and the Association of National Advertisers' (ANA) #SeeHer initiative will p… https://t.co/2wtYxKJVuO
    Tweet 117: RT @ZoeListerJones: Tonight’s an all new Life In Pieces and it’s directed by my ride or die @nataliaanderson!!!… https://t.co/2LPfmyLWrY
    Tweet 118: RT @MarenMorris: Hot damn! Woke up from my post-wisdom teeth haze to find out I’m up for 4 @ACMawards ! So honored, especially for the Dear…
    Tweet 119: RT @KelseaBallerini: Ohhhhh goodness. Incredible. Thank you thank you thank you. #female https://t.co/1ZTYjNfQeF
    Tweet 120: RT @KeithUrban: ACMs...... HOLY SMOKES!!!!! MAD LOVE TO U ALL THIS MORNING  FOR THESE INCREDIBLE NOMINATIONS. I’M EXTREMELY GRATEFUL!!!!!!!…
    Tweet 121: RT @ACMawards: Congratulations to this year’s #ACMawards Video of the Year nominees:
    “Black” - @DierksBentley
    “It Ain’t My Fault” - @Brothe…
    Tweet 122: RT @ACMawards: Please give a round of applause to this year’s #ACMawards Entertainer of the Year nominees: @Jason_Aldean, @GarthBrooks, @Lu…
    Tweet 123: .@ChrisStapleton, @ThomasRhett, @mirandalambert and more are all nominated for awards at Country Music's Party of t… https://t.co/Vm1vXRUDYJ
    Tweet 124: The Queen of Country, @Reba, is returning to host the 53rd #ACMawards on Sunday, April 15 at 8/7c. Here are a few o… https://t.co/Iqzz6Gql01
    Tweet 125: RT @survivorcbs: It’s time! #Survivor https://t.co/YPk6cGWrUA
    Tweet 126: RT @CBSThisMorning: TOMORROW: The nominees for the 2018 @ACMawards will be announced live by the one-and-only, @Reba! 
    
    Watch on @CBS in ou…
    Tweet 127: RT @thegoodfight: From the set design and costumes to hair and makeup, the production quality is truly next-level. Take a peek inside the u…
    Tweet 128: RT @LivinBiblically: The fun continues on Facebook! The #LivingBiblically cast is live to talk about tonight’s premiere. Tune in here: http…
    Tweet 129: RT @KevinCanWaitCBS: Can you get all the way through these #KevinCanWait bloopers without laughing?! @KevinJames,@LeahRemini and the rest o…
    Tweet 130: RT @ACMawards: That’s right! @Reba is headed to @CBSThisMorning on Thursday, March 1 to announce this year’s #ACMAwards' nominees. Tune in…
    Tweet 131: RT @ScorpionCBS: You can't hack your way to a 197 IQ, but you are well on your way with these Genius Facts from #TeamScorpion! 💻 You can be…
    Tweet 132: RT @SuperiorDonuts: You can always count on @DavidKoechner for a laugh! Did your favorite Tush moment make the list? Catch a new #SuperiorD…
    Tweet 133: RT @TheTalkCBS: TODAY: We loved them together then &amp; we love seeing them together now! Welcome back to the show @THESaraGilbert​'s good fri…
    Tweet 134: RT @thegoodfight: As foundations begin to crumble, our characters struggle to make sense of this new dystopian world. The cast teases what'…
    Tweet 135: #LivingBiblically's @linzkraft and @jrfergjr appeared on @KCBS's Facebook Live this morning, talking all about what… https://t.co/4RebcHuuMQ
    Tweet 136: RT @CBSSports: Introducing CBS Sports HQ, a New 24/7 Direct-to-Consumer Streaming Network for Sports News, Highlights, &amp; Analysis.
    
    Stream…
    Tweet 137: RT @CBSBigBrother: It’s down to the final 5 celebrity Houseguests, and anyone could take home the grand prize! Tune in NOW to watch the #BB…
    Tweet 138: RT @startrekcbs: Binge the entire first season of #StarTrekDiscovery. All episodes now streaming exclusively on CBS All Access: https://t.c…
    Tweet 139: RT @thegoodfight: #TheGoodFight returns in 1 week. Season 2 premieres Sunday, March 4. https://t.co/nomCao1GWp https://t.co/BOn6bOe9Tb
    Tweet 140: RT @thegoodfight: This is our new favorite thing. Christine Baranski debuted #TheGoodFight the Musical on @colbertlateshow last night! 🎵🎤…
    Tweet 141: RT @LivinBiblically: Confession time: have YOU ever hit the "close door" button in an elevator while somebody was approaching? The cast of…
    Tweet 142: RT @CBSEyeSpeak: Mark your calendars! #CBSEyeSpeak kicks off March 14 with The EYE Speak Summit. Follow our page for more details! https://…
    Tweet 143: RT @CBSEyeSpeak: Proud to announce a new CBS initiative, promoting female empowerment and developing the next generation of leaders through…
    Tweet 144: RT @LivinBiblically: When you're living by the Bible, it's good to have a priest and a rabbi on call (provided they answer their phones, th…
    Tweet 145: RT @thegoodfight: Chicago lawyers are being hunted and the world is going insane. 
    
    The new season of #TheGoodFight premieres Sunday, March…
    Tweet 146: Ready for some larger than life competition? This new series from @MarkBurnettTV will premiere in summer 2018.… https://t.co/gDXHLdIJ5v
    Tweet 147: With tournament dreams on the line, make sure to stream these college basketball matchups on CBS All Access:… https://t.co/SGkYUZrQWB
    Tweet 148: RT @LivinBiblically: While Chip's sticking to the Bible's original rules, the cast of #LivingBiblically has given them a more modern makeov…
    Tweet 149: Casting News! Peter Mark Kendall, Michael Gaston, Greg Wise, Rade Šerbedžija, Zack Pearlman, and Keye Chen join the… https://t.co/GFob2KrD8H
    Tweet 150: RT @BullCBS: The verdict is in...#Bull is the perfect Valentine! ❤️ Happy #ValentinesDay! https://t.co/poEejI4AnC
    Tweet 151: RT @NoActivityCBS: Car 27 reporting: Season 2 of #NoActivity coming soon!
    
    Binge season one now on CBS All Access: https://t.co/yvxoQMeyhN…
    Tweet 152: RT @LivinBiblically: Against all odds (and the advice of his God Squad), Chip is determined to live life by the Good Book. Think you could…
    Tweet 153: RT @thegoodfight: Christine Baranski reflects upon the spectacular metamorphosis of her character in #TheGoodFight's first season. Revisit…
    Tweet 154: RT @startrekcbs: Binge the entire first season of #StarTrekDiscovery. All 15 episodes now streaming on CBS All Access: https://t.co/lKLaptP…
    Tweet 155: RT @SuperiorDonuts: Looking for a #Valentine? Tush is here to help you land your dream date just in time for the day of love! #SuperiorDonu…
    Tweet 156: RT @CBSBigBrother: The pressure is on as the Houseguests battle each other for victory in the first HOH competition. Stream the season prem…
    Tweet 157: RT @startrekcbs: Sunday, this season's epic journey reaches its final reckoning. Catch up before the season finale: https://t.co/4Ea5wpmAep…
    Tweet 158: Are you a sucker for jaw-dropping talent competitions? Announcing The World's Best, a first-of-its-kind new global… https://t.co/fagCMklm2Z
    Tweet 159: RT @BullCBS: Tonight, one of these 3 TAC employees will end up incarcerated. Who do you think it will be? Tune into a new episode of #Bull…
    Tweet 160: RT @thegoodfight: There's no season like lawyer season. #TheGoodFight returns March 4, exclusively on CBS All Access. https://t.co/XAlrg1nB…
    Tweet 161: RT @thegoodfight: The acclaimed series returns in 1 month. #TheGoodFight is back Sunday, March 4. https://t.co/5BNLTYRd8p https://t.co/yInP…
    Tweet 162: .@KeshaRose's emotional performance with @CyndiLauper, @AndraDayMusic, @BebeRexha and @Camila_Cabello brought the a… https://t.co/qpgS2e9ir0
    Tweet 163: Broadway legend Patti LuPone paid tribute to Sir Andrew Lloyd Webber with a show-stopping rendition of the iconic s… https://t.co/GxqLtUzhFP
    Tweet 164: Relive an entire night's worth of big wins, live performances and many more inspiring moments from The 60th Annual… https://t.co/CH7mJ4Y3QT
    Tweet 165: RT @swatcbs: ✨@TheTalkCBS' @TheRealEve and @ShemarMoore brought some serious heat to the GRAMMYs red carpet: https://t.co/WYTITKkLQd https:…
    Tweet 166: The all-star collaboration between @Rihanna, @DJKhaled and @brysontiller brought the party to the #GRAMMYs with a s… https://t.co/jTv5QbtgUp
    Tweet 167: RT @TheTalkCBS: All the #redcarpet looks from the 60th annual #GRAMMYs! Did you catch @TheRealEve's sparkly two-piece suit? Diamond accents…
    Tweet 168: Go backstage with the celebs and see which stars celebrated the big night together! Here are the moments you missed… https://t.co/rgQTXRQyLg
    Tweet 169: 🎶 @DearEvanHansen star Ben Platt gave a soaring performance of a classic Broadway hit. Watch him perform 'Somewhere… https://t.co/HcuizFWo02
    Tweet 170: 🎸 @LittleBigTown, @U2, @BrunoMars and @Pink are just a few of the superstar performers that lit up the stage at The… https://t.co/2v0MNYMItG
    Tweet 171: Don't miss country stars Emmylou Harris and @ChrisStapleton perform a moving rendition of @TomPetty's classic song,… https://t.co/GxoIW97XIu
    Tweet 172: RT @survivorcbs: Get excited! 🙌 It’s finally time to meet the castaways of #Survivor: Ghost Island ☠ https://t.co/xQQ8ROhKbM https://t.co/P…
    Tweet 173: Country stars @EricChurch, @BrothersOsborne and @MarenMorris performed a touching tribute dedicated to the victims… https://t.co/YCIq9i70fd
    Tweet 174: Missed the Man vs. Beast #SuperBowlCommercials showdown? Catch up now to find out which entertaining ad was named t… https://t.co/SlyO0n7yMX
    Tweet 175: RT @rikimae: Ok, that one makes me cry #superbowlcommercials
    Tweet 176: If this doesn’t make you cry, nothing will. #SuperBowlCommercials https://t.co/UjFf3xr4so
    Tweet 177: RT @maverickkr: #superbowlcommercials the frogs were great, I'd forgotten about that one.
    Tweet 178: If you haven't voted yet, it's not too late! Choose your favorite here: https://t.co/s4pEIF5IX8… https://t.co/VCa1YHDyYy
    Tweet 179: RT @5691e44f3b66446: I am having so much fun ...the commercials are awesome #superbowlcommercials
    Tweet 180: RT @LittleShelbyMae: Love watching the #superbowlcommercials all the animal ones are my favorite!!
    Tweet 181: RT @PatHale915: Doritos time machine was great!   #superbowlcommercials
    Tweet 182: Never make a goat angry. #SuperBowlCommercials https://t.co/xRJssdAErk
    Tweet 183: RT @MsLindsieStarr: That dang goat! #Doritos #superbowlcommercials
    Tweet 184: Do you remember this Super Bowl commercial featuring @KevinHart4real playing an overprotective father?… https://t.co/ejLnTEG5ug
    Tweet 185: RT @dRoy: Homework will take a break for now. #superbowlcommercials on cbs is on 👌
    Tweet 186: RT @Mixxxalv: Watching #superbowlcommercials let the best win!
    Tweet 187: Super Bowl Greatest Commercials 2018 hosts @7BOOMERESIASON and @DanielaRuah are moments away from announcing the tw… https://t.co/cKM1WXPc9u
    Tweet 188: It's Man vs. Beast as these classic Super Bowl spots face off tonight on Super Bowl Greatest Commercials 2018! Tune… https://t.co/GcoSAcfeUd
    Tweet 189: Tonight, get a first look at #ChrisElliott starring in the new @AvosfromMexico Super Bowl Commercial on… https://t.co/WMEjsZfgdg
    Tweet 190: Join @NCISLA's @DanielaRuah and @7BOOMERESIASON on Super Bowl Greatest Commercials 2018, tonight at 8/7c on CBS and… https://t.co/nzVu1FCRc5
    Tweet 191: Everyone knows the best part of the Super Bowl is the commercials! Find out how to tune in and vote for your favori… https://t.co/KGDYl9oF98
    Tweet 192: Throw it back to this classic Michael Jordan vs. Larry Bird spot tonight on Super Bowl Greatest Commercials 2018! W… https://t.co/g2H4zV7wIN
    Tweet 193: Super Bowl Greatest Commercials 2018 airs tonight! Cast your vote now, then tune in and vote for the winner live at… https://t.co/h4jGpUheP5
    Tweet 194: Remember when @robinthicke and Lil Wayne (@liltunechi) joined jazz legends on stage at the 51st #GRAMMYs? Watch thi… https://t.co/2l8gBdOrli
    Tweet 195: Tomorrow, catch a sneak peek of the incredible @TiffanyHaddish in her first Super Bowl Commercial for @Groupon on… https://t.co/RlehujNvFN
    Tweet 196: Rapper @Logic301 delivered a poignant message of hope to conclude his powerful performance with @alessiacara and… https://t.co/elSqZExRtN
    Tweet 197: Even our host @JKCorden was starstruck when Broadway icon Patti LuPone performed at The #GRAMMYs! https://t.co/YJhNBllFh6
    Tweet 198: Childish Gambino serenaded the crowd with his smooth vocals in this mesmerizing performance. Watch him perform 'Ter… https://t.co/TkWZP7PS22
    Tweet 199: .@OfficialSting and @shaggy757 joined together to bring their upbeat reggae vibes to Madison Square Garden. Watch t… https://t.co/iqq0DbM1IR
    Tweet 200: Superstar @Pink left it all on the stage last night in a simply stunning performance that brought the crowd to thei… https://t.co/B9Tj5INcNu
    Tweet 201: Wall Street is still trying to figure out which President Trump it'll get https://t.co/CYmOnyOIZW https://t.co/fFPsFhDTEu
    Tweet 202: The papacy has ruled for 2,000 years. Now go back to the beginning of the Pope’s rise to power. #Pope premieres ton… https://t.co/gI3SrvhHav
    Tweet 203: President Trump brags about his party's "5-0" record in House special elections https://t.co/vPSCfNDYeF https://t.co/imHxAcVfCB
    Tweet 204: Northeast prepares for its 3rd winter storm in the past 10 days https://t.co/JoTRQetMZY https://t.co/Jc6CQmUv7G
    Tweet 205: "Frankly, I just don't see how any senator can vote to weaken the regulations on Wall Street banks." Democratic Sen… https://t.co/UElyMlyCl7
    Tweet 206: Customers of a restaurant and a pub in the UK have been urged to wash their clothes as a precaution after traces of… https://t.co/D6mdjVheg6
    Tweet 207: "We can't do what we did with Iran and let off the pressure and then just watch the behavior go in the wrong direct… https://t.co/Z8knp3MEam
    Tweet 208: US Defense Secretary James Mattis warns Syria against using chemical weapons, doubts Russian missile claims… https://t.co/SS2g8Prnq1
    Tweet 209: 66% of Millennials have nothing saved for retirement, a new report shows. Of those who do save, most have less than… https://t.co/LV5n4lhDKy
    Tweet 210: Video games do not teach people to become shooters in real life | By Lindsay Grace via @CNNOpinion… https://t.co/To5tJkefdd
    Tweet 211: Local law says that "every head of household residing in the city limits is required to maintain a firearm" in Kenn… https://t.co/Nf0Lm30jOd
    Tweet 212: 11 tax deductions and other ways to cut your 2017 tax bill https://t.co/jq7skCHDUf https://t.co/EJkXr8jlW9
    Tweet 213: Oprah says she's "not running for office,"  but her advice for someone who might is "do not give your energy to the… https://t.co/n2iTxgYjrl
    Tweet 214: 11 tax deductions and other ways to cut your 2017 tax bill https://t.co/QQ1nZJEfRJ https://t.co/D2DABt7h1W
    Tweet 215: Anthony C. Acevedo, the first Mexican-American to register as a Holocaust survivor, was a World War II medic and on… https://t.co/IQ14hNQBWr
    Tweet 216: Sen. Elizabeth Warren: “I am not running for president in 2020” https://t.co/0dy8gxtdRw https://t.co/M99yjYnqvR
    Tweet 217: Sen. Elizabeth Warren: "I want the President to succeed … but I am very worried he is going to go into these negoti… https://t.co/Q3DQPYof0Y
    Tweet 218: Sen. Elizabeth Warren: "I am very glad to see this administration move toward a diplomatic approach to North Korea”… https://t.co/bFt8iWwzKq
    Tweet 219: GOP Sen. Ron Johnson of Wisconsin: “It’s just incredibly important to, I would say, ratchet up the sanctions agains… https://t.co/9tuearvy9f
    Tweet 220: This photographer snaps cancer-stricken kids as superheroes https://t.co/bRrWqqIk7r https://t.co/5MFEqEFbMw
    Tweet 221: Barbie unveiled dolls based on real-life figures including Amelia Earhart, Frida Kahlo, Katherine Johnson and Chloe… https://t.co/Ss8N6WEbY4
    Tweet 222: 66% of Millennials have nothing saved for retirement, a new report shows. Of those who do save, most have less than… https://t.co/3MB6jMLDON
    Tweet 223: Inspired by her travels, this artist paints stunning pictures on tea bags https://t.co/O8piOHSRFL https://t.co/HZSii2dacL
    Tweet 224: Local law says that "every head of household residing in the city limits is required to maintain a firearm" in Kenn… https://t.co/2R3nT6dWU7
    Tweet 225: "They could divorce. They could own property. They had many rights that women in subsequent civilizations didn't ha… https://t.co/5897yT64XL
    Tweet 226: These high-performance cars, which are priced in the millions, can't be legally driven on public roads… https://t.co/xF15GOjWIb
    Tweet 227: "Dumpster fire" is in the Merriam-Webster dictionary now, defined as "an utterly calamitous or mismanaged situation… https://t.co/oMS9hLU8Lx
    Tweet 228: Child marriage is declining, a new UNICEF report says - but 12 million girls are still married in childhood every y… https://t.co/DAUZ1yVebf
    Tweet 229: Scientists unravel the mystery of the Roman "gate to hell," which was thought to belch a "breath of death" that cau… https://t.co/WJbHlsGepb
    Tweet 230: China's largely ceremonial parliament endorses a constitutional amendment, paving the way for President Xi Jinping… https://t.co/RCTC01CJUz
    Tweet 231: Find out who and what is making big waves in the worlds of tech, media and innovation. Sign up for PACIFIC, CNN’s n… https://t.co/V8Ln7DhjHo
    Tweet 232: Facebook robots may one day follow you around at home https://t.co/urKEOzSzgF https://t.co/t1dARIPBBS
    Tweet 233: Fake news travels at a markedly faster rate on Twitter than accurate information, a new study finds… https://t.co/kpbDwphXbN
    Tweet 234: Uber says it has started using self-driving trucks to haul freight in Arizona https://t.co/r5bQPtepzj https://t.co/Fdxg8DUHHI
    Tweet 235: Former White House press secretary Jay Carney says the role he once held has become unrecognizable under President… https://t.co/K3qLcWnL4R
    Tweet 236: "Black Panther" to cross $1 billion worldwide in less than a month https://t.co/wJzaL36egw https://t.co/4uiuQSt2CG
    Tweet 237: Over 1.3 million live animals and plants, and a similar number of animal skins, have been exported from Africa to A… https://t.co/fHImyRVV9l
    Tweet 238: NASA spacecraft Juno has collected new data on its mission to Jupiter revealing some of the swirling inner mysterie… https://t.co/Tm73ZWcpn9
    Tweet 239: Bones found on a remote Pacific island almost eight decades ago likely are those of pioneering pilot Amelia Earhart… https://t.co/MtqRDZ4kgB
    Tweet 240: Russian internet trolls created an anti-Hillary Clinton video game called "Hilltendo" and tried to make it go viral… https://t.co/aRZHkrFLDL
    Tweet 241: President Trump is looking at adding impeachment lawyer to the White House team, The New York Times reports… https://t.co/clGPoY680u
    Tweet 242: US Attorney General Jeff Sessions says judges blocking President Trump's policies is "unconstitutional"… https://t.co/6FPpSHLqpd
    Tweet 243: "A Wrinkle in Time" breaks through with a new kind of heroism, writes Van Jones for @CNNOpinion… https://t.co/nHKWj0zLmn
    Tweet 244: Washington, save partisan fighting for actual partisan issues — not DACA, writes Fernand Fernandez for @CNNOpinion… https://t.co/PkLyK4GUMq
    Tweet 245: The CEO of dating app Bumble says she pays no mind to Tinder, her biggest competitor and former employer… https://t.co/vOQrSvoZoV
    Tweet 246: US worries over China's expanding role in Africa https://t.co/JdQ485Nc35 https://t.co/HoPZkfx31V
    Tweet 247: Oprah says she was inspired to help the Florida school shooting survivors fight for gun control because their dedic… https://t.co/SHC9psBXC6
    Tweet 248: President Trump appears to mock "presidential" behavior: "That's easy" https://t.co/zvDdpiNUy1
    Tweet 249: US Secretary of State Rex Tillerson will resume his normal schedule Sunday after canceling his Saturday schedule of… https://t.co/8g4GLb4WHv
    Tweet 250: President Trump discusses the death penalty for drug dealers: "I don't think we should play games ... These people… https://t.co/yZwcNUrreM
    Tweet 251: President Trump responds to chants of "Build the wall" at a rally in Pennsylvania: "We are building the wall, 100%" https://t.co/Oy2HVMcGjp
    Tweet 252: Ireland seals Six Nations Championship title with four-try victory over Scotland https://t.co/uvZ1EZ067i https://t.co/AL5mTXS040
    Tweet 253: President Trump: Even President Ronald Reagan "was not great" on trade https://t.co/UETvww4jNW
    Tweet 254: President Trump announces his 2020 campaign slogan: "Keep America Great!" https://t.co/hKeK7AB2h6
    Tweet 255: President Trump: "I'd love to beat Oprah" in the 2020 presidential election https://t.co/t30yRIq0zd
    Tweet 256: Turkish court frees 2 journalists on bail in trial of Cumhuriyet opposition paper https://t.co/Un1EUty2p6 https://t.co/mEJQZ19oc5
    Tweet 257: President Trump hits his predecessors on North Korea: "They had their shot and all they did was nothing" https://t.co/CIkhp64uW2
    Tweet 258: President Trump on negotiations with North Korea: "This should have been handled, by the way, over the last 30 year… https://t.co/dKCUO0NRzD
    Tweet 259: President Trump touts his new steel and aluminum tariffs during a rally for Pennsylvania GOP special election candi… https://t.co/Q3E8P3zFBy
    Tweet 260: Will Supreme Court Justice Anthony Kennedy retire and be Republicans' Senate savior?https://t.co/kr6I66I97I https://t.co/RnI7IY8i3j
    Tweet 261: LGBTQ groups cry discrimination as Republican Sen. Mike Lee reintroduces religious freedom bill… https://t.co/HqPsXmxcYf
    Tweet 262: President Trump's trade moves may be of no help in Pennsylvania special election https://t.co/11inyamR9f https://t.co/vJMpYrhEkU
    Tweet 263: US Secretary of State Rex Tillerson changes tune after President Trump accepts meeting with North Korean leader Kim… https://t.co/kFsksZqn0u
    Tweet 264: President Trump's estranged adviser Steve Bannon tells a far-right gathering in France that they should handle accu… https://t.co/X2jidYv3xU
    Tweet 265: Steven Spielberg's "Ready Player One" to have world premiere at SXSW https://t.co/AzahlUPzxB https://t.co/P6cND1J2cs
    Tweet 266: Attorney General Jeff Sessions tests limits of immigration powers with asylum moves https://t.co/B8AfIWFrDD https://t.co/8D4RnpFuT3
    Tweet 267: The week President Trump rewarded enemies and punished friends https://t.co/h9ZNBoxZ9q https://t.co/wgTo39OKvt
    Tweet 268: "The darkness is there to show you your light." Oprah Winfrey gives advice to Americans feeling jaded amid increasi… https://t.co/bNTETFcemm
    Tweet 269: Why is the US rolling back this high-return global investment? https://t.co/PHS5LIGmDj (via @CNNOpinion) https://t.co/nPp5q40N5r
    Tweet 270: Jackie Kennedy looked like a movie star on her wedding day. But it wasn’t the wedding of her dreams. Go inside… https://t.co/oCzygC50Dv
    Tweet 271: Read CNN's interview with Stormy Daniels https://t.co/YIfuKhzX3Q https://t.co/HJOLk05OHs
    Tweet 272: Florida adopts a wide package of gun laws, while Washington stalls https://t.co/F9zXo17teo https://t.co/dZlwaY7P7X
    Tweet 273: How porn star Stormy Daniels could impact the Russia investigation https://t.co/JoYkWOmiOx https://t.co/4UiUdSq7pE
    Tweet 274: Adwoa Aboah announces next "Gurls Talk" event in Ghana https://t.co/TI9WQ2BMZT https://t.co/dWDF0fpn3X
    Tweet 275: Why China's footprint in Africa worries the US https://t.co/p1XrRgGA01 https://t.co/JXSDwDzDQ5
    Tweet 276: Porn star Stormy Daniels says Trump scandal has been good for business https://t.co/CWliGpCPF5 https://t.co/VXZew4GxnF
    Tweet 277: "Black Panther" to cross $1 billion worldwide in less than a month https://t.co/HKIJkqfpvn https://t.co/q6pnbyPeJ8
    Tweet 278: A woman rode a horse into a dance club in Miami Beach https://t.co/e1ipW7Mih8 https://t.co/2MVq8Z8dEl
    Tweet 279: A lawsuit accusing Harvard of capping Asian-American admissions could be tried this summer https://t.co/c6DLxTGmlp https://t.co/oSb5ywGPjT
    Tweet 280: Oprah's advice for Americans feeling jaded amid increasing political partisanship, gun control debates and inner ci… https://t.co/GBDRMAEiaY
    Tweet 281: Complaining about the change to Daylight Saving Time? You're in the minority. https://t.co/oJnAcfmYqQ https://t.co/WQWU2PUCBj
    Tweet 282: Find out who and what is making big waves in the worlds of tech, media and innovation. Sign up for PACIFIC, CNN’s n… https://t.co/7cYXQ71LuB
    Tweet 283: This is the one big difference in the story about the Nashville mayor's affair https://t.co/bBA4azjyYj (via… https://t.co/h45W9IdVgr
    Tweet 284: UK ministers are holding an emergency meeting as investigators probe the attempt to kill a former Russian double ag… https://t.co/ahpM64kxYH
    Tweet 285: JUST IN: A man is barricaded in an apartment in Pomona, California, after fatally shooting an officer and wounding… https://t.co/sc3x1MrBDM
    Tweet 286: Here are some reads you missed during a busy news week https://t.co/S1PcV7ztqp https://t.co/6btlUSS4XN
    Tweet 287: God bless these doctors in Canada https://t.co/ulphJ72jda (via @CNNOpinion) https://t.co/uSfkIpUIWc
    Tweet 288: Job growth is strong, but it probably won't save Republicans in the midterms https://t.co/7lYLdRReVK | Analysis by… https://t.co/8hgxVcWYhs
    Tweet 289: What President Xi Jinping's power play means for China's economy https://t.co/VtvRiBYcsu https://t.co/Nf7bGSiQx4
    Tweet 290: Will hitting China with tariffs kill India's solar dreams?https://t.co/XpemciO5nU https://t.co/ZHv7Rv416B
    Tweet 291: This photographer snaps cancer-stricken kids as superheroes https://t.co/fFC6pucyUH https://t.co/LFXLQw84eE
    Tweet 292: Barbie unveiled dolls based on real-life figures including Amelia Earhart, Frida Kahlo, Katherine Johnson and Chloe… https://t.co/XTcr37oUtS
    Tweet 293: Inspired by her travels, this artist paints stunning pictures on tea bags https://t.co/nz5dDAlxC4 https://t.co/Zg6KvN3iWF
    Tweet 294: "They could divorce. They could own property. They had many rights that women in subsequent civilizations didn't ha… https://t.co/aU6DxQeeaR
    Tweet 295: "Dumpster fire" is in the Merriam-Webster dictionary now, defined as "an utterly calamitous or mismanaged situation… https://t.co/60GM8iM45n
    Tweet 296: Thousands of dead starfish were found washed up on British shores https://t.co/oIGXDH5mhI https://t.co/5VqOSsvuqr
    Tweet 297: Cecil the lion "suffered incredible cruelty for at least 10 hours" before he died, according to a lion researcher's… https://t.co/nojuVmY238
    Tweet 298: 2,000 years ago, people watched as animals mysteriously died at the entrance to a grotto in modern-day Turkey that… https://t.co/6fU1KnQyxK
    Tweet 299: The kind of floods we've only seen during storms might someday become regular events as sea levels rise due to glob… https://t.co/IMDfqxFTkh
    Tweet 300: Fake news travels at a markedly faster rate on Twitter than accurate information, a new study finds… https://t.co/NZoj9PtRDr
    Tweet 301: Friends: excited for the long weekend!
    Me: https://t.co/vPjwIGbbM4
    Tweet 302: RT @lrnrd: THIS THREAD 🍿🔥🙏🏻👏💯 https://t.co/vhfH4ETTC0
    Tweet 303: RT @ChloeCondon: Booth recruiter 🧔: ...and we really value diversity!
    
    Me 👧🏼: Do you have any women's sized shirts?
    
    🧔: Unfortunately no, t…
    Tweet 304: Amazing hosts for upcoming @CSSconfeu 💖 https://t.co/pmtGtflroP
    Tweet 305: Still wondering what companies that have less than 10% women (let alone POC) across the entire organisation (not on… https://t.co/yIyuV9WkMS
    Tweet 306: Women’s Day isn’t an arbitrary date. 
    
    It should be an ongoing celebration, empowerment and inclusion of all women… https://t.co/5EJ63WQXL6
    Tweet 307: RT @butwhoiskat: I’ll be speaking (for the first time) next Thursday, at WDYK, on the ethics of design.
    
    Wanna come support me?
    
    https://t.…
    Tweet 308: @Double_Days Hey! Unfortunately, that’s not me :)
    Tweet 309: RT @void_daddy_: the future isnt female. the future is nonbinary it's genderfluid it's queer and it's trans it's natives rising up against…
    Tweet 310: RT @soniagupta504: I'm starting to really understand the importance of psychological safety on developer teams. 
    
    With it, you have a team…
    Tweet 311: RT @vboykis: Somewhere in the multiverse, this HN exists. https://t.co/bdk0sZeucF
    Tweet 312: @ohhoe blocked because the truth was too stingy, the follow up james damore story
    Tweet 313: RT @triketora: such a useful resource for founders to be able to find the anti-harassment policies and points of contact for the vc firms t…
    Tweet 314: Hi friends! I’m looking for new opportunities to put my multidisciplinary skills to good use within a diverse organ… https://t.co/Chl7QaEgSe
    Tweet 315: @ANZ_AU You can email me at hi@thefox.is.
    Tweet 316: @Mandy_Kerr Back at you, Mandy! You’re amazing! ✨
    Tweet 317: @happycrappie thank you!
    Tweet 318: RT @shailjapatel: Read women.
    Cite women.
    Credit women.
    
    Teach women.
    Publish women.
    Present women.
    
    Acknowledge women.
    Award women.
    Amplif…
    Tweet 319: RT @EricaJoy: Men! Celebrate #InternationalWomensDay by taking direct action:
    🌟 Tell the women you work with how much you get paid.
    🌟 If yo…
    Tweet 320: Another release of @jsconfeu tickets is happening in a few minutes and you don’t want to miss out. https://t.co/MX1CYi6kiK
    Tweet 321: Everything about this—”the absence of no is not consent.”
    
    https://t.co/58usW9cFxH
    Tweet 322: Don’t try to shame me for your lack of professionalism and overconfident behaviour. 
    
    Tl:dr; don’t work for @ANZ_AU.
    Tweet 323: I told her how offensive and exclusionary those things were. Her reply:
    
    ”I'm disappointed with your interpretation… https://t.co/diQiVBdLF0
    Tweet 324: I got emailed by someone at @ANZ_AU who not only insinuated that I’m after ”fancy titles” after I said I want to dr… https://t.co/kpCyQY3i1z
    Tweet 325: @theroyals hey, thanks but that doesn’t sound like me, and I find a bunch of the language on your site problematic.
    Tweet 326: I think I’m going to collect “the best” team pages and toxic statements I see when looking for a new role. Already seen so many.
    
    🤢
    Tweet 327: @Catharz Thanks! I’ve interviewed in the past, but unfortunately, it didn’t fulfil my requirements on diversity and inclusion.
    Tweet 328: @s_mcleod 👋🏻 thanks! unfortunately, I think I’m mostly looking to steer away from traditional front-end roles to so… https://t.co/VJ18W89w3A
    Tweet 329: @stephenbelanger 👋🏻 Elastic has been recommended to me several times now. I would be keen to chat to someone. Would… https://t.co/YOKEU5HWVf
    Tweet 330: @mikerapp haha, please feel free to. I was inspired by wonderful @lrnrd :)
    Tweet 331: @d2kagw @camplexer 👋🏻  sure! would you be able to drop a line at hi@thefox.is with more details, please?
    Tweet 332: @breskeby hey, thanks! unfortunately, I don’t want to pursue the path of a front/JS developer so it wouldn’t be a good fit :)
    Tweet 333: @superhighfives Which park is this?!
    Tweet 334: @maxvoltar Heh, got too excited and didn’t notice the “US remote”. Forever sad face of not willing to relocate to San Francisco. 
    
    😭
    Tweet 335: @maxvoltar @goabstract Oh hi hello 👋🏻 https://t.co/3xme16xAIz
    Tweet 336: RT @amberdawn: The product I've been working on launched today! @begin is a new way to handle tasking - create tasks, pick the one you're f…
    Tweet 337: @sara_ann_marie I have failed at this multiple times before, but I try my best to step out of my white cis privileg… https://t.co/0Ooul9WLgq
    Tweet 338: Having the audacity to talk about diversity and have a bunch of cis, able-bodied, white women is just eternally amazing.
    Tweet 339: The all white women panel is the new all-male panel. https://t.co/xDGHNGWC49
    Tweet 340: RT @sara_ann_marie: We're working on getting @noyougoshow in front of new audiences—where do y'all think we should be featured? Orgs we sho…
    Tweet 341: @dantoml *nods*
    
    Or no careers page, no benefits, no process explained, nothing.
    Tweet 342: I don’t know if the majority of men realise the magnitude of mental energy needed to find a job as a member of an u… https://t.co/xCUFjAE2UD
    Tweet 343: Looking for a job makes me realise how little has changed in the tech industry as I browse through countless almost… https://t.co/HbM885Ah1k
    Tweet 344: RT @fox: 🚨👋🏻 Hi everyone! I’m searching for new opportunities. 
    
    I’m looking for multidisciplinary product, front-end and leadership roles.…
    Tweet 345: Oh look, it’s the show off your women to avoid getting called out day!
    Tweet 346: @DevelopSean Thanks Sean! I appreciate it.
    Tweet 347: @mr_lundis Ah. It sounded like it’s only on site. I’ll have a look!
    Tweet 348: @mr_lundis Sounds interesting but I’m in Australia and not relocating :)
    Tweet 349: @peres Thank you, but I’m not looking to relocate. :)
    Tweet 350: @limbera I didn’t see it at all! Would you mind sharing some details through email?
    Tweet 351: @tjalve Nope :)
    Tweet 352: Hi European friends 👋🏻 I’m looking for a new job. Find more details below. ⬇️ https://t.co/3xme16Pc79
    Tweet 353: @z_rose Hey! You can email hi at https://t.co/vah0lKuzCY.
    Tweet 354: What I’ve learned over the years is that often denying lack of inclusion is much worse than being honestly exclusionary.
    Tweet 355: You are right, there are 6% (7/101 instructors) of women instructors judging based on https://t.co/2JyuREYsYu (pron… https://t.co/JtEKL6cacH
    Tweet 356: RT @sara_ann_marie: 4. If an organizer doesn't know where to find more diverse speakers, ask them why not. How do they find speakers now? W…
    Tweet 357: RT @sara_ann_marie: Specific things I do:
    1. Ask organizers what they're doing to ensure a diverse lineup. 
    2. Listen closely to the answer…
    Tweet 358: RT @CharlotteAlter: Bumble expels anyone who uses hate speech, has a 0 tolerance harassment policy, regularly removes trolls, and is now ba…
    Tweet 359: @brianleroux @benschwarz Paging @jahoni 😂
    Tweet 360: @twholman How do I live up to this now 😂 thank you!
    Tweet 361: @DevelopSean indeed! Thank you!
    Tweet 362: @DevelopSean @InVisionApp Anyone I could talk to? :)
    Tweet 363: @jennschiffer Beautiful art boobs. Congrats! 👏🏻
    Tweet 364: Also, if you have recommendations or can introduce me to good people, please do! 🙇🏻‍♀️
    Tweet 365: @malchata Thank you, Jeremy! 🌸
    Tweet 366: @Folletto It could be an option, could you introduce me to anyone I can talk to? :)
    Tweet 367: @frameshiftllc Oh, I have never seen any examples of such transparency. Thank you for sharing!
    Tweet 368: @emanuil_tolev Thank you Emanuil!
    Tweet 369: @saltnburnem Thank you for your kind words!
    Tweet 370: @andymcmillan 😭
    
    Thank you, Andy! 💐
    Tweet 371: 🚨👋🏻 Hi everyone! I’m searching for new opportunities. 
    
    I’m looking for multidisciplinary product, front-end and le… https://t.co/0T3EgXuCMK
    Tweet 372: @jenistyping Yep, I’m seeing a lot of people charging and making decent money, not giving back to the community or… https://t.co/rNdb3tSwf5
    Tweet 373: RT @uveavanto: Saw these, thought I would share. https://t.co/XXisevHRrz
    Tweet 374: RT @QVWCmelb: ‘We all care. How do we translate that into action? The data shows that we won’t get to gender pay equity for another 50 year…
    Tweet 375: RT @QVWCmelb: ‘In 2018 we shouldn’t be talking about STEM education. We should be talking about algorithm design. It’s a human right and sh…
    Tweet 376: RT @MandelaSH: 9 in 10 VCs are White and Asian men
    9 in 10 VC-backed founders are White and Asian men
    Coincidence? 🤔
    Tweet 377: RT @ikasliwal: A hard truth about women's spaces in Slack https://t.co/NBjHCGD3yN
    Tweet 378: RT @aahunsberger: Words matter. I have been called all these things. 
    
    I am not aggressive. I am assertive - I know what i want. 
    I am not…
    Tweet 379: When your neighbourhood coffee shop plays Jungle (loud) and brings you bottomless filter. ❤️
    
    https://t.co/8pq3hOhZF7
    Tweet 380: Claiming care and commitment to diversity and inclusion, charging for tickets, having sponsors but NOT PAYING speak… https://t.co/WVbD6YOw9r
    Tweet 381: Any diversity and inclusion event not paying their speakers is perpetuating systems of injustice and the precedent… https://t.co/N7mARvzGJf
    Tweet 382: Evergreen mood. https://t.co/46SrAc4PKY
    Tweet 383: RT @projectinclude: Did you know: the pay gap between white women and women of color is the fastest growing income inequality there is, acc…
    Tweet 384: @ohhoe I have called out that guy multiple times and all I got was hostile snark. I wouldn’t expect much from him a… https://t.co/3fzrczcPHj
    Tweet 385: I think my level of stress and exhaustion warrants a silent yoga and meditation retreat
    Tweet 386: @vaurorapub @sw All I ask is one day when I don’t have to bend backwards to prove I’m “worthy” and “exceptional”
    Tweet 387: @decryption The NYT article is horrible and doing a massive disservice. Nonetheless, research about the founders is… https://t.co/2ofMnMgbDT
    Tweet 388: @sw Truth. I got used to the term, but I should ditch it.
    Tweet 389: @notwaldorf The amount of white, privileged feminism I see is as disturbing as Twitter feminism bios. Sob.
    
    Actions and allyship or leave.
    Tweet 390: @ezyjules ughhhhhhhhh
    Tweet 391: This, with a special dedication for white, privileged, non-intersectional feminists. https://t.co/UwPxpfnUej
    Tweet 392: @decryption You also haven’t done your homework to check who actually the person running Lumineer is, because if yo… https://t.co/Flb13H9M65
    Tweet 393: @decryption Hi Anthony. I’m really disappointed in the Sizzle. You’ve written a horribly harmful piece of content a… https://t.co/kpTJ9RhcI3
    Tweet 394: RT @notwaldorf: At some point in my 30s I became a party pooper feminist. Inclusion is more than straight white women. It’s trans women, bl…
    Tweet 395: Reality comes at you fast. https://t.co/FYdgWyz6Ww
    Tweet 396: @ellehardytweets @sw has been advocating &amp; working against exclusionary, toxic tech culture for decades (here’s a p… https://t.co/NbdO7yxlYw
    Tweet 397: RT @meelijane: Know any women looking to make a jump into UX? @codelikeagirlau and @academyxi are joining forces to offer a sweet $10K, 10-…
    Tweet 398: @superhighfives I need beach in my life stat
    Tweet 399: Number of times I got asked to provide D&amp;I advice: countless.
    
    Number of times I gave actionable D&amp;I advice for fre… https://t.co/81UejD06DI
    Tweet 400: Bono apologized and said he was "furious" after former employees of a charity he co-founded accused managers of bul… https://t.co/N1Rkdf8Qjo
    Tweet 401: The program was intended to reintegrate soldiers into civilian life. Friday’s shooting at a California veterans hom… https://t.co/sHRPr4f1Sb
    Tweet 402: GAAAAAAHHH! https://t.co/RY2PFkqU5Y
    Tweet 403: RT @MujMash: Around this time 2001 the Taliban destroyed the Bamiyan Buddhas.
    Their frames still stand tall in the valley.
    “The Buddhas wer…
    Tweet 404: Trump’s seemingly unstoppable series of erratic moves has helped cast China as the more stable power in Asia https://t.co/w0KT9IUqe7
    Tweet 405: We want to hear how college students handle consent for sexual intimacy in relationships and encounters https://t.co/FRVOuFyp23
    Tweet 406: President Xi Jinping has set China on course to follow his hard-line authoritarian rule far into the future https://t.co/ZNBzhGP6KQ
    Tweet 407: RT @malika_andrews: It’s selection Sunday! After a season riddled with setbacks, Louisville believes it has done enough to have its name ca…
    Tweet 408: Stephen Bannon and Marine Le Pen met in a symbolic embrace in France. It was not clear whether the encounter would… https://t.co/VOsQfDpp1a
    Tweet 409: Charmaine Pruitt, a black evangelical, thought she had found a spiritual home at a white-majority church. Then came… https://t.co/2jhkWQoKpo
    Tweet 410: Trump has staked his reputation as a deal maker on the presumption that he can personally achieve what no other pre… https://t.co/uoydgUbADf
    Tweet 411: In an effort to better understand the top chief executives — where they’re coming from and how they deal with the c… https://t.co/0F3Errf58a
    Tweet 412: RT @motokorich: For Asia, Trump's willingness to meet Kim Jong-un averts--at least for now--the threat of nuclear conflict. Then again, the…
    Tweet 413: Modern Love: "From the moment our daughter wakes to the moment she goes to sleep, we are engaged in a waltz with pe… https://t.co/1GLvH9ouWQ
    Tweet 414: Here are some of the highlights of the past week https://t.co/gSh2fxDPD8
    Tweet 415: Bono Apologizes as Accusations of Abuse Hit Charity He Co-Founded https://t.co/qhfChDlonA
    Tweet 416: Stephen Bannon, the exiled Trump adviser, is done wrecking the American establishment. His next mission? Training E… https://t.co/5vhAFL7cWy
    Tweet 417: President Trump is in discussions with a lawyer who represented Bill Clinton during the impeachment process about j… https://t.co/yQWnJirGpP
    Tweet 418: The Kushner and Trump families have been stepping up their real estate collaborations, blurring the line between fa… https://t.co/wjU7n0cpbW
    Tweet 419: Contributing Op-Ed Writer: YouTube, the Great Radicalizer https://t.co/DYj4CcKsD9
    Tweet 420: Emily Warren Roebling was not an engineer. But when her husband fell ill, she oversaw the construction and completi… https://t.co/7Ggw4LPRdk
    Tweet 421: Midwestern farmers could become collateral damage from retaliation against American tariffs https://t.co/fePAD3pZ7T
    Tweet 422: “These regulations were written with human blood.” Safety rules adopted after the Deepwater Horizon disaster are a… https://t.co/lRxMuPECzk
    Tweet 423: Prime Minister Justin Trudeau of Canada has appointed the first permanent female leader of the Royal Canadian Mount… https://t.co/R4AReaxMxU
    Tweet 424: In Chile, a Billionaire Takes the Reins From a Socialist, Again https://t.co/wiFa3t6bzK
    Tweet 425: The worlds of drug marketing and TV entertainment are blurring. Take a look at a recent episode of "black-ish." https://t.co/KvyFSyAIUO
    Tweet 426: Donald Trump’s election shook one Ohio man so badly he swore off all political news. But what are the ethics of ign… https://t.co/srVJ0dS1Pn
    Tweet 427: On College Basketball: As N.C.A.A. Field Comes Together, Integrity Is on the Bubble https://t.co/ai34W61OpN
    Tweet 428: ‘Saturday Night Live’ Parodies ‘The Bachelor’ With Another Kind of Breakup https://t.co/32ZNhuG68s
    Tweet 429: G.O.P. Rushed to Pass Tax Overhaul. Now It May Need to Be Altered. https://t.co/8GV1VyeNY1
    Tweet 430: Vows: From Crayons to Chemo, He’s Back by Her Side https://t.co/khs9VanuQm
    Tweet 431: City Kitchen: A Take on Shepherd’s Pie, Straight From the Farm https://t.co/H5d5NlBJgC
    Tweet 432: How Do You Teach People to Love Difficult Music? https://t.co/pGIZHTO278
    Tweet 433: At the Hayden Planetarium, a Joyride Across the Cosmos https://t.co/WNi6IWjjJp
    Tweet 434: In Opinion
    
    Op-Ed contributors John McCain and Angelina Jolie write, "Around the world, there is profound concern t… https://t.co/vVHUdWVe0o
    Tweet 435: President Trump will convene with a group of leaders who have criticized his statements and policies on immigration https://t.co/nv9rMEIPon
    Tweet 436: China’s Xi Wins Constitutional Backing for New Strongman Era https://t.co/M3WNKiEPkv
    Tweet 437: If you’re worried about the stock market, find someone to give you a hug. Let us explain. https://t.co/Ba4T4wFNT7
    Tweet 438: "I'm a little rusty, but it doesn't matter," Serena Williams said to the crowd. "I'm just out here on this journey… https://t.co/7oUOEYBDZa
    Tweet 439: Some British schools are encouraging riskier play to build resilience and grit https://t.co/cCkagvGTHw
    Tweet 440: What to Pack for 36 Hours in Auckland https://t.co/R8wXEH8Hpw
    Tweet 441: Ida B. Wells is considered by historians to have been the most famous black woman in the U.S. during her lifetime.… https://t.co/aoFduRNlyR
    Tweet 442: Make this pot of red beans and rice https://t.co/ABJjSxsnMw https://t.co/yjmbAmYqM9
    Tweet 443: Michael B. Jordan says his company will adopt inclusion riders, the diversity effort championed by Frances McDorman… https://t.co/MNdDFuMVIt
    Tweet 444: Mired in poverty not long ago, Ghana’s economic growth is on track to outpace India’s https://t.co/lJpEBXU7cn
    Tweet 445: Sensing a power grab in China by President Xi Jinping, Chinese students in the U.S. are showing signs of activism f… https://t.co/QImdvZPUZp
    Tweet 446: From everyone cares and everything matters to no one cares and nothing matters. Here are 25 songs that tell us wher… https://t.co/wufZ5kGonc
    Tweet 447: Clocks spring forward this weekend. Florida never wants them to fall back. https://t.co/AhBfQGFt3t
    Tweet 448: Fake news on Twitter is 70% more likely to be shared than the truth, researchers at MIT found https://t.co/PDx3HOmyfE
    Tweet 449: Peter Thiel is leaving Silicon Valley. Will others? https://t.co/0srODDzsRg
    Tweet 450: The sickle cell mutation arose 7,300 years ago in just one person in West Africa, scientists say. Its purpose: a sh… https://t.co/qpYVUarVZJ
    Tweet 451: A Horse Walked Into a Miami Beach Nightclub. Now Nobody Else Can. https://t.co/kaw0IMzBga
    Tweet 452: Amazon says it has figured out why its digital assistant, Alexa, has been mysteriously laughing for some users https://t.co/zE07cpOBNV
    Tweet 453: Violence Strikes a Veterans Program That Strove to Prevent It https://t.co/hvCn5WOiIc
    Tweet 454: “We need your help,” a waitress in a small town whispered to our reporter. Here's what's happening in Madison, Indi… https://t.co/aLpZcnQdEX
    Tweet 455: Adam Rippon: "I kind of have a feeling that I’ve used the Olympics to get myself to meet the world" https://t.co/prxur7Ha0U
    Tweet 456: It’s True: False News Spreads Faster and Wider. And Humans Are to Blame. https://t.co/1tpUTE8QV5
    Tweet 457: “It’s sort of disheartening at first to realize how much we humans are responsible," one MIT researcher said of fal… https://t.co/6oZXCa5Frp
    Tweet 458: “Somebody must show that the Afro-American race is more sinned against than sinning, and it seems to have fallen up… https://t.co/Qq9akMXkLy
    Tweet 459: After reigning atop the music industry for a decade, Taylor Swift suddenly seems under review https://t.co/3TNDXjLIby
    Tweet 460: Over the years Frida Kahlo has been associated with a number of unlikely products. But what about a Barbie doll? https://t.co/hmiWCz6VQN
    Tweet 461: California Police Officer Killed by Barricaded Gunman, Officials Say https://t.co/r6b4hyKEL3
    Tweet 462: The excavation of Rome’s newest subway line has been the gift that keeps on giving for archaeologists https://t.co/cU0L1MwWU1
    Tweet 463: Opinion: Eyes Here: Weekend Reads From the Opinion Section https://t.co/ANpEESonA0
    Tweet 464: An elaborate system has developed to silence women who level accusations against powerful men. One of those women i… https://t.co/4Dmxpjsgi8
    Tweet 465: The Unbearable Cuteness of Maternity Overalls https://t.co/pfOpGxSbfr
    Tweet 466: Here’s Cardi B’s “Bodak Yellow” and your 3 minutes 44 seconds of frankness in a year of feminist pandering https://t.co/YVZ4Ae8ODr
    Tweet 467: The ground around San Francisco Bay is sinking to meet the rising sea https://t.co/XOOxtmWNq4
    Tweet 468: Conversations about sex and consent are especially urgent on college campuses. So let’s talk.… https://t.co/C3TkIYwa4O
    Tweet 469: A Ballet Score? Not Such a Stretch for the Pet Shop Boys https://t.co/3nWpUjVpc3
    Tweet 470: In Opinion
    
    Op-Ed contributor Rania Abouzeid writes, "Were his old extremist ideas merely dormant, awaiting activat… https://t.co/QVqFjvudsA
    Tweet 471: Saturday playlist: the 12 new songs you need to hear https://t.co/JENBAmj4JQ
    Tweet 472: Trump’s seemingly unstoppable series of erratic moves has helped cast China as the more stable power in Asia https://t.co/TuHgOajWOE
    Tweet 473: Millennials are going to great lengths to bring plant life into their homes and work spaces https://t.co/5gpI7gA4mW
    Tweet 474: In Opinion
    
    The editorial board writes, "Nonvoters aren’t protesting anything; they’re just putting their lives and… https://t.co/1PbDgInQwv
    Tweet 475: Two California police officers were shot, one fatally, by a gunman who was barricaded in an apartment https://t.co/PC18ZeDaig
    Tweet 476: Stephen Bannon, the exiled Trump adviser, is done wrecking the American establishment. His next mission? Training E… https://t.co/MqLhX2OI2M
    Tweet 477: President Trump is strongly considering Christopher P. Liddell, a former executive at Microsoft and General Motors,… https://t.co/3w1XRZiSTh
    Tweet 478: RT @jackhealyNYT: This fascinating @samdolnick story makes me want to ask this guy many questions. 
    
    Like, fair enough to ignore D.C. intri…
    Tweet 479: RT @nytopinion: The list the world needs now. https://t.co/YPlFLe2B4j
    Tweet 480: President Trump is in discussions with a lawyer who represented Bill Clinton during the impeachment process about j… https://t.co/aWitB4FliI
    Tweet 481: Behind-the-scenes activity signals a deepening of the Trump-Kushner business relationship. It also poses potential… https://t.co/IemVcizZ3p
    Tweet 482: In Opinion
    Senior correspondent and editor Susan Chira writes, "Many women, those who grew up wealthy and those who… https://t.co/KByAgYbcr0
    Tweet 483: Midwestern farmers could become collateral damage from retaliation against American tariffs https://t.co/oE1FMsiFPD
    Tweet 484: Drew Houston, Dropbox’s chief executive, is set to become the next bona fide tech billionaire https://t.co/Pf0FERTf56
    Tweet 485: Trump has staked his reputation as a deal maker on the presumption that he can personally achieve what no other pre… https://t.co/xvzgwWZlYI
    Tweet 486: One year ago today, the invasion of the toddlers https://t.co/Tdov9NdtSA
    Tweet 487: After a year on the job, Betsy DeVos is learning that leaving school policymaking to the states means not always ge… https://t.co/3lEO7t9XH4
    Tweet 488: Some British schools are encouraging riskier play to build resilience and grit https://t.co/c49lN1yiyO
    Tweet 489: Sensing a power grab in China by President Xi Jinping, Chinese students in the U.S. are showing signs of activism f… https://t.co/NkhlyGV6UJ
    Tweet 490: Ida B. Wells was a 30-year-old newspaper editor living in Memphis when she began her anti-lynching campaign… https://t.co/BA3cg7uVPK
    Tweet 491: Man United’s 2-1 triumph at Old Trafford was the perfect José Mourinho victory: one marked by an undeniable effecti… https://t.co/zXGylSVY48
    Tweet 492: Since 1851, obituaries in The New York Times have been dominated by white men. Now, we're adding the stories of mor… https://t.co/jXa3k5EjLP
    Tweet 493: The cement giant Lafarge stayed in Syria when other companies left. Now it is accused of paying off ISIS militants… https://t.co/LnXsqgDwCb
    Tweet 494: “These regulations were written with human blood.” Safety rules adopted after the Deepwater Horizon disaster are a… https://t.co/KMpCc20r1U
    Tweet 495: “He has the privilege of constructing a world in which very little of what he doesn’t have to deal with gets throug… https://t.co/RNbZdS04KW
    Tweet 496: President Trump will convene with a group of leaders who have criticized his statements and policies on immigration https://t.co/YnZwLDUj9V
    Tweet 497: Modern Love wants to hear about how college students handle sexual consent. Share your story. https://t.co/jrU1PE87Tg
    Tweet 498: Mired in poverty not long ago, Ghana’s economic growth is on track to outpace India’s https://t.co/4KWVGXkPxH
    Tweet 499: Donald Trump’s victory shook him. Badly. And so Erik Hagerman developed his own eccentric experiment: He swore that… https://t.co/r36KEL2SDm



```python
#Create Dataframe
sentiments_df=pd.DataFrame.from_dict(sentiments)
```


```python
#Save to .csv
sentiments_csv= sentiments_df[['Media Source','Date','Text','Compound','Positive','Neutral','Negative','Tweet Count']]#restuctur
sentiments_csv['Media Source'] = sentiments_csv['Media Source'].map(lambda x: x.lstrip('@'))
sentiments_csv.head()  #check
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Media Source</th>
      <th>Date</th>
      <th>Text</th>
      <th>Compound</th>
      <th>Positive</th>
      <th>Neutral</th>
      <th>Negative</th>
      <th>Tweet Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>BBC</td>
      <td>Sun Mar 11 17:02:03 +0000 2018</td>
      <td>🐑 Ewe won't believe how this farmer revives fr...</td>
      <td>0.3818</td>
      <td>0.206</td>
      <td>0.794</td>
      <td>0.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>BBC</td>
      <td>Sun Mar 11 16:55:19 +0000 2018</td>
      <td>RT @BBCMOTD: John Motson in his element during...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>BBC</td>
      <td>Sun Mar 11 16:00:04 +0000 2018</td>
      <td>By 2050, 30-50% of all species could be headin...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.0</td>
      <td>3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>BBC</td>
      <td>Sun Mar 11 15:01:05 +0000 2018</td>
      <td>🗑🚀 Tomorrow’s space scientists will have to de...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.0</td>
      <td>4</td>
    </tr>
    <tr>
      <th>4</th>
      <td>BBC</td>
      <td>Sun Mar 11 14:43:09 +0000 2018</td>
      <td>RT @5liveSport: For the final time...\n\nThe l...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.0</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
</div>




```python
sentiments_csv.to_csv("News Mood.csv")
```


```python
# assign legend colors
news_outlet_colors={"BBC": "lightblue",
             "CBS":"green", 
             "CNN":"red",
             "Fox":"blue",
             "nytimes": "yellow"}
```


```python
#Sentiment Analysis of Media Tweets Plot
sns.set()
plt.figure(figsize = (15,10))
plt.xlabel("Tweets Ago",fontweight='bold')
plt.ylabel("Tweet Polarity",fontweight='bold')
plt.title("Sentiment Analysis of Media Tweets (%s)" % (time.strftime("%m/%d/%Y")),fontweight='bold')
plt.xlim(102,-2, -1)
plt.ylim(-1,1)
for newsoutlet in news_outlet_colors.keys():
    df = sentiments_csv[sentiments_csv['Media Source'] == newsoutlet]
    sentiment_analysis = plt.scatter(df["Tweet Count"],df["Compound"], label = newsoutlet, color = news_colors[newsoutlet], edgecolor = "black", s=125)
plt.legend(bbox_to_anchor = (1,1), title = 'Media Sources')    
plt.show()
sentiment_analysis.figure.savefig('SentimentAnalysis.png')
```


    <matplotlib.figure.Figure at 0x1a1b265438>



![png](output_8_1.png)



```python
#Mean scores by news outlet
scoresbyoutlet=sentiments_csv.groupby("Media Source")["Compound"].mean()
scoresbyoutlet 
```




    Media Source
    BBC        0.136925
    CBS        0.329151
    CNN       -0.033611
    Fox        0.161544
    nytimes    0.053644
    Name: Compound, dtype: float64




```python
x_axis=np.arange(len(scoresbyoutlet))
```


```python
# Create bar chart
sns.set()

plt.figure(figsize = (10,8))
for newsoutlet in news_outlet_colors.keys():
    df = sentiments_csv[sentiments_csv['Media Source'] == newsoutlet]
#     print (news_colors[newsoutlet])
   
    overall_media_sentiment = plt.bar(x_axis,scoresbyoutlet, color = {"lightblue","green", "yellow", "red", "blue"}, label = newsoutlet, edgecolor = "black")
    
# plt.bar(x_axis,scoresbyoutlet, label = newsoutlet, color = news_colors[newsoutlet], edgecolor = "black")
plt.ylim(-.1, .45)
plt.ylabel("Tweet Polarity",fontweight='bold')
plt.axhline(y=0, color = 'black') #adds a horizontal line at zero
for i, v in enumerate(scoresbyoutlet):
    plt.text(i-.3, v+.035, str(v).format(), color='black', fontweight='bold')
plt.title("Overall Media Sentiment Based on Twitter (%s)" % (time.strftime("%m/%d/%Y")),fontweight='bold')
x_labels = ["BBC", "CBS", "CNN", "Fox", "NYT"]
x_locations = [value for value in np.arange(6)]  #tick locations
plt.xticks(x_locations, x_labels)
plt.show()
plt.savefig('Overall Media Sentiment Based on Twitter.png')
```


![png](output_11_0.png)

