---
Alias: US
cssClasses: cards, cards-cover, cards-2-3, table-max, max
obsidianUIMode: preview
banner: "https://upload.wikimedia.org/wikipedia/en/thumb/a/a4/Flag_of_the_United_States.svg/1200px-Flag_of_the_United_States.svg.png?20151118161041"
banner_y: 0.51205
---
WHERE contains(meta(Country).path, this.file.path)
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



FLATTEN {
    "Book": Author,
    "Audiobook": Author,
    "Podcast": Author,
    "eBook": Author,
    "AcademicBook": Author,
    "AcademicArticle": Author,
    "GraphicNovel": Author,
    "Movie": Director,
    "TV": Director,
    "Game": Director
}[Medium] AS Creator



FLATTEN link(file.link, Alias) + " by " + Creator AS P_BookByAuthor
FLATTEN choice(contains(tags, "Favorite"), "💛", "") AS P_FavoriteIcon
SORT DateFinished desc
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




# ---
TABLE without id 
      ("![](" + Cover + ")") AS "Cover",
      P_BookByAuthor + P_MediumIcon + P_FavoriteIcon AS "Title",
      Q_YearRating + Q_PagesHours + " || " AS "Time and Place",      
      R_Dates AS "Dates",
      S_ReadingTime + " || " + S_ReadingSpeed AS "Reading Time"
      
FROM "Book Log"

WHERE Medium != null 
SORT DateFinished desc

FLATTEN choice(contains(tags, "Favorite"), "💛", "") AS P_FavoriteIcon

FLATTEN {
    "Book": " 📖 ",
    "Audiobook": " 🎧 ",
    "Podcast": " 📻 ",
    "eBook": " 📃 ",
    "GraphicNovel": " 💬 ",
    "AcademicBook": " 📚🎓 ",
    "AcademicArticle": " 📰 "
}[Medium] AS P_MediumIcon

FLATTEN {
    "Book": Author,
    "Audiobook": Author,
    "Podcast": Author,
    "eBook": Author,
    "AcademicBook": Author,
    "AcademicArticle": Author,
    "GraphicNovel": Author,
    "Movie": Director,
    "TV": Director,
    "Game": Director
}[Medium] AS OneAuthor

FLATTEN link(file.link, Alias) + " by " + Author AS P_BookByAuthor


FLATTEN DateStarted + " — " + DateFinished AS R_Dates

FLATTEN choice(((DateFinished - DateStarted).days = 1), "1 day",  choice((DateFinished != DateStarted), (DateFinished - DateStarted).days + " days", "0 days" )) AS S_ReadingTime


FLATTEN {
"Book": round(choice((DateFinished = DateStarted), Length, (Length / (DateFinished - DateStarted).days)), 1) + " p/d",
"Audiobook": round(choice((DateFinished = DateStarted), Length, (Length / (DateFinished - DateStarted).days)), 2) + " h/d",
"Podcast": round(choice((DateFinished = DateStarted), Length, (Length / (DateFinished - DateStarted).days)), 2)+ " h/d",
"eBook": round(choice((DateFinished = DateStarted), Length, (Length / (DateFinished - DateStarted).days)), 1) + " p/d",
"GraphicNovel": round(choice((DateFinished = DateStarted), Length, (Length / (DateFinished - DateStarted).days)), 1) + " p/d",
"AcademicBook": round(choice((DateFinished = DateStarted), Length, (Length / (DateFinished - DateStarted).days)), 1) + " p/d",
"AcademicArticle": round(choice((DateFinished = DateStarted), Length, (Length / (DateFinished - DateStarted).days)), 1) + " p/d"
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




# aaaaaaaaaaaaaaaa






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



FLATTEN DateStarted + " — " + DateFinished AS R_Dates
FLATTEN {
"Book": round(choice((DateFinished = DateStarted), Length, (Length / (DateFinished - DateStarted).days)), 1) + " p/d",
"Audiobook": round(choice((DateFinished = DateStarted), Length, (Length / (DateFinished - DateStarted).days)), 2) + " h/d",
"Podcast": round(choice((DateFinished = DateStarted), Length, (Length / (DateFinished - DateStarted).days)), 2)+ " h/d",
"eBook": round(choice((DateFinished = DateStarted), Length, (Length / (DateFinished - DateStarted).days)), 1) + " p/d",
"GraphicNovel": round(choice((DateFinished = DateStarted), Length, (Length / (DateFinished - DateStarted).days)), 1) + " p/d",
"AcademicBook": round(choice((DateFinished = DateStarted), Length, (Length / (DateFinished - DateStarted).days)), 1) + " p/d",
"AcademicArticle": round(choice((DateFinished = DateStarted), Length, (Length / (DateFinished - DateStarted).days)), 1) + " p/d",
"Movie": round(choice((DateFinished = DateStarted), (Length/60), ((Length/60) / (DateFinished - DateStarted).days)), 2)+ " h/d",
"TV": round(choice((DateFinished = DateStarted), (Length/60), ((Length/60) / (DateFinished - DateStarted).days)), 2)+ " h/d"
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

FLATTEN join(map(choice(typeof(Country) = "array", Country, list(Country)), (c) => link(
      meta(c).path, {
AD: "🇦🇩",
AE: "🇦🇪",
AF: "🇦🇫",
AG: "🇦🇬",
AI: "🇦🇮",
AL: "🇦🇱",
AM: "🇦🇲",
AO: "🇦🇴",
AQ: "🇦🇶",
AR: "🇦🇷",
AS: "🇦🇸",
AT: "🇦🇹",
AU: "🇦🇺",
AW: "🇦🇼",
AX: "🇦🇽",
AZ: "🇦🇿",
BA: "🇧🇦",
BB: "🇧🇧",
BD: "🇧🇩",
BE: "🇧🇪",
BF: "🇧🇫",
BG: "🇧🇬",
BH: "🇧🇭",
BI: "🇧🇮",
BJ: "🇧🇯",
BL: "🇧🇱",
BM: "🇧🇲",
BN: "🇧🇳",
BO: "🇧🇴",
BQ: "🇧🇶",
BR: "🇧🇷",
BS: "🇧🇸",
BT: "🇧🇹",
BV: "🇧🇻",
BW: "🇧🇼",
BY: "🇧🇾",
BZ: "🇧🇿",
CA: "🇨🇦",
CC: "🇨🇨",
CD: "🇨🇩",
CF: "🇨🇫",
CG: "🇨🇬",
CH: "🇨🇭",
CI: "🇨🇮",
CK: "🇨🇰",
CL: "🇨🇱",
CM: "🇨🇲",
CN: "🇨🇳",
CO: "🇨🇴",
CR: "🇨🇷",
CU: "🇨🇺",
CV: "🇨🇻",
CW: "🇨🇼",
CX: "🇨🇽",
CY: "🇨🇾",
CYMRU: "🏴󠁧󠁢󠁷󠁬󠁳󠁿",
CZ: "🇨🇿",
DE: "🇩🇪",
DJ: "🇩🇯",
DK: "🇩🇰",
DM: "🇩🇲",
DO: "🇩🇴",
DZ: "🇩🇿",
EC: "🇪🇨",
EE: "🇪🇪",
EG: "🇪🇬",
EH: "🇪🇭",
ENG: "🏴󠁧󠁢󠁥󠁮󠁧󠁿",
ER: "🇪🇷",
ES: "🇪🇸",
ET: "🇪🇹",
FI: "🇫🇮",
FJ: "🇫🇯",
FK: "🇫🇰",
FM: "🇫🇲",
FO: "🇫🇴",
FR: "🇫🇷",
GA: "🇬🇦",
GB: "🇬🇧",
GD: "🇬🇩",
GE: "🇬🇪",
GF: "🇬🇫",
GG: "🇬🇬",
GH: "🇬🇭",
GI: "🇬🇮",
GL: "🇬🇱",
GM: "🇬🇲",
GN: "🇬🇳",
GP: "🇬🇵",
GQ: "🇬🇶",
GR: "🇬🇷",
GS: "🇬🇸",
GT: "🇬🇹",
GU: "🇬🇺",
GW: "🇬🇼",
GY: "🇬🇾",
HK: "🇭🇰",
HM: "🇭🇲",
HN: "🇭🇳",
HR: "🇭🇷",
HT: "🇭🇹",
HU: "🇭🇺",
ID: "🇮🇩",
IE: "🇮🇪",
IL: "🇮🇱",
IM: "🇮🇲",
IN: "🇮🇳",
IO: "🇮🇴",
IQ: "🇮🇶",
IR: "🇮🇷",
IS: "🇮🇸",
IT: "🇮🇹",
JE: "🇯🇪",
JM: "🇯🇲",
JO: "🇯🇴",
JP: "🇯🇵",
KE: "🇰🇪",
KG: "🇰🇬",
KH: "🇰🇭",
KI: "🇰🇮",
KM: "🇰🇲",
KN: "🇰🇳",
KP: "🇰🇵",
KR: "🇰🇷",
KW: "🇰🇼",
KY: "🇰🇾",
KZ: "🇰🇿",
LA: "🇱🇦",
LB: "🇱🇧",
LC: "🇱🇨",
LI: "🇱🇮",
LK: "🇱🇰",
LR: "🇱🇷",
LS: "🇱🇸",
LT: "🇱🇹",
LU: "🇱🇺",
LV: "🇱🇻",
LY: "🇱🇾",
MA: "🇲🇦",
MC: "🇲🇨",
MD: "🇲🇩",
ME: "🇲🇪",
MF: "🇲🇫",
MG: "🇲🇬",
MH: "🇲🇭",
MK: "🇲🇰",
ML: "🇲🇱",
MM: "🇲🇲",
MN: "🇲🇳",
MO: "🇲🇴",
MP: "🇲🇵",
MQ: "🇲🇶",
MR: "🇲🇷",
MS: "🇲🇸",
MT: "🇲🇹",
MU: "🇲🇺",
MV: "🇲🇻",
MW: "🇲🇼",
MX: "🇲🇽",
MY: "🇲🇾",
MZ: "🇲🇿",
NA: "🇳🇦",
NC: "🇳🇨",
NE: "🇳🇪",
NF: "🇳🇫",
NG: "🇳🇬",
NI: "🇳🇮",
NL: "🇳🇱",
NO: "🇳🇴",
NP: "🇳🇵",
NR: "🇳🇷",
NU: "🇳🇺",
NZ: "🇳🇿",
OM: "🇴🇲",
PA: "🇵🇦",
PE: "🇵🇪",
PF: "🇵🇫",
PG: "🇵🇬",
PH: "🇵🇭",
PK: "🇵🇰",
PL: "🇵🇱",
PM: "🇵🇲",
PN: "🇵🇳",
PR: "🇵🇷",
PS: "🇵🇸",
PT: "🇵🇹",
PW: "🇵🇼",
PY: "🇵🇾",
QA: "🇶🇦",
RE: "🇷🇪",
RO: "🇷🇴",
RS: "🇷🇸",
RU: "🇷🇺",
RW: "🇷🇼",
SA: "🇸🇦",
SB: "🇸🇧",
SC: "🇸🇨",
SCOT: "🏴󠁧󠁢󠁳󠁣󠁴󠁿",
SD: "🇸🇩",
SE: "🇸🇪",
SG: "🇸🇬",
SH: "🇸🇭",
SI: "🇸🇮",
SJ: "🇸🇯",
SK: "🇸🇰",
SL: "🇸🇱",
SM: "🇸🇲",
SN: "🇸🇳",
SO: "🇸🇴",
SR: "🇸🇷",
SS: "🇸🇸",
ST: "🇸🇹",
SV: "🇸🇻",
SX: "🇸🇽",
SY: "🇸🇾",
SZ: "🇸🇿",
TC: "🇹🇨",
TD: "🇹🇩",
TF: "🇹🇫",
TG: "🇹🇬",
TH: "🇹🇭",
TJ: "🇹🇯",
TK: "🇹🇰",
TL: "🇹🇱",
TM: "🇹🇲",
TN: "🇹🇳",
TO: "🇹🇴",
TR: "🇹🇷",
TT: "🇹🇹",
TV: "🇹🇻",
TW: "🇹🇼",
TZ: "🇹🇿",
UA: "🇺🇦",
UG: "🇺🇬",
UM: "🇺🇲",
US: "🇺🇸",
UY: "🇺🇾",
UZ: "🇺🇿",
VA: "🇻🇦",
VC: "🇻🇨",
VE: "🇻🇪",
VG: "🇻🇬",
VI: "🇻🇮",
VN: "🇻🇳",
VU: "🇻🇺",
WF: "🇼🇫",
WS: "🇼🇸",
YE: "🇾🇪",
YT: "🇾🇹",
ZA: "🇿🇦",
ZM: "🇿🇲",
ZW: "🇿🇼"
}[meta(c).display]
  )), ", ") AS Q_FLAG

```

