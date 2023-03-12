---
Alias: JP
cssClasses: cards, cards-cover, cards-2-3, table-max, max
obsidianUIMode: preview
banner: https://upload.wikimedia.org/wikipedia/en/thumb/9/9e/Flag_of_Japan.svg/1200px-Flag_of_Japan.svg.png?20111003030759
---

# Things from here
```dataview
TABLE without id 
      ("![](" + Cover + ")") AS "Cover",
      P_BookByAuthor + P_MediumIcon + P_FavoriteIcon AS "Title",
      Q_YearRating + Q_PagesHours + " || " AS "Time and Place",      
      R_Dates AS "Dates",
      "(" + S_ReadingTime + " || " + S_ReadingSpeed + ")" AS "Reading Time"
      
FROM "Book Log" OR "Y Movies and Games" OR "0 Currently Reading" OR "Set Aside" 

WHERE econtains(meta(Country).path, this.file.path)
FLATTEN link(file.link, Alias) + " by " + Author AS P_BookByAuthor
FLATTEN choice(contains(tags, "Favorite"), "💛", "") AS P_FavoriteIcon

FLATTEN {
    "Book": " 📖 ",
    "Audiobook": " 🎧 ",
    "Podcast": " 📻 ",
    "eBook": " 📃 ",
    "AcademicBook": " 📚🎓 ",
    "AcademicArticle": " 📰 ",
    "GraphicNovel": " 💬 ",
	"Movie": " 🎞️ ",
	"TV": " 📺 ",
	"Game": " 🎮 "
}[Medium] AS P_MediumIcon

FLATTEN DateStarted + " — " + DateFinished AS R_Dates

FLATTEN choice(((DateFinished - DateStarted).days = 1), "1 day",  choice((DateFinished != DateStarted), (DateFinished - DateStarted).days + " days", "0 days" )) AS S_ReadingTime


FLATTEN {
"Book": round(choice((DateFinished = DateStarted), Length, (Length / (DateFinished - DateStarted).days)), 1) + " p/d",
"Audiobook": round(choice((DateFinished = DateStarted), Length, (Length / (DateFinished - DateStarted).days)), 2) + " h/d",
"Podcast": round(choice((DateFinished = DateStarted), Length, (Length / (DateFinished - DateStarted).days)), 2)+ " h/d",
"eBook": round(choice((DateFinished = DateStarted), Length, (Length / (DateFinished - DateStarted).days)), 1) + " p/d",
"GraphicNovel": round(choice((DateFinished = DateStarted), Length, (Length / (DateFinished - DateStarted).days)), 1) + " p/d",
"AcademicBook": round(choice((DateFinished = DateStarted), Length, (Length / (DateFinished - DateStarted).days)), 1) + " p/d",
"AcademicArticle": round(choice((DateFinished = DateStarted), Length, (Length / (DateFinished - DateStarted).days)), 1) + " p/d",
"Movie": round(choice((DateFinished = DateStarted), round((Length/60), 1), (Length / (DateFinished - DateStarted).days)), 1) + " h/d",
"TV": round(choice((DateFinished = DateStarted), round((Length/60), 1), (Length / (DateFinished - DateStarted).days)), 1) + " h/d",
"Game": round(choice((DateFinished = DateStarted), round((Length/60), 1), (Length / (DateFinished - DateStarted).days)), 1) + " h/d"
}[Medium] AS S_ReadingSpeed

FLATTEN Year + " || " + " ⭐ " + Rating + " || " AS Q_YearRating

FLATTEN {
    "Book": Length + " p",
    "Audiobook": Length + " h",
    "Podcast": Length + " h",
    "eBook": Length + " p",
    "GraphicNovel": Length + " p",
    "AcademicBook": Length + " p",
    "AcademicArticle": Length + " p",
	"Movie": round((Length/60), 1) + " h",
	"TV": round((Length/60), 1) + " h",
	"Game": round((Length/60), 1) + " h"
}[Medium] AS Q_PagesHours



```
