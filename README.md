# naviki-api
Naviki is a cycling application used by challenges organizer to record participants gps traces. Despite not documented naviki seems to have an open API to access the challenges data. To help anyone to develop custom tools or widgets to animate the challenge here is a brief documentation of the API as it is in 20 June 2020. 

Contest informations
------------------------
URL: https://www.naviki.org/naviki/api/v5/Contest/2/findContest/{CONTESTID}

Where {CONTESTID} needs to be replaced by the unique id of the contest. To find this id you may inspect the naviki's contests dedicated pages to see which id is used in your case.

example of use:

``console
https://www.naviki.org/naviki/api/v5/Contest/2/findContest/51084
``

returns:
```css
{
  "package":"org.naviki",
  "uid":51084,"title":"Challenge Métropolitain du Vélo 2020",
  "description":"La Métropole Européenne de Lille organise le Challenge Métropolitain du vélo du 1er au 30 juin 2020.\r\nLe vélo est vertueux pour l’environnement, bon pour la santé et sûr du point de vue sanitaire pour les trajets quotidiens. Habitants de la métropole, tenez-vous prêts, sortez votre vélo et tous en selle!",
  "link":null,
  "rulesPageLink":null,
  "imageUrl":"https://www.naviki.org/fileadmin/contests/Lille/challenge-velo_2020.jpg",
  "headerImageUrl":null,
  "useAllWays":null,
  "maxTeamMembers":null,
  "isPublicContest":null,
  "topLevelsAvailable":null,
  "teamsAvailable":true,
  "teamMessagesAvailable":null,
  "isSendTeamMessagesAllowed":null,
  "memberCategoriesAvailable":null,
  "teamCategoriesAvailable":null,
  "isCreateTeamsEnabled":null,
  "globalHeatmapUrl":null,
  "isHeatmapEnabled":null,
  "isTeamModificationAllowed":null,
  "isFinished":null,
  "isStarted":null,
  "categories":[],
  "totalKm":322491.9,                /// The total number of km done. This may be different from the sum of team's totalKm
  "totalKmInsideBoundary":308303.2,  /// The total number of km done that fits the contest constraint
  "availableRankings":null,
  "controlQuestion":null
 }

```


Teams related informations
------------------------------
URL: https://www.naviki.org/naviki/api/v5/Contest/findTeams/{CONTESTID}

Where {CONTESTID} needs to be replaced by the unique id of the contest. 

Parameters: offset, limit

example of use:

``console
https://www.naviki.org/naviki/api/v5/Contest/findTeams/51084?offset=10&limit=30
``


returns:
```css
{
    "teams": [
        {
            "uid": 79874,                             /// The team unique id
            "name": "#MyTeamName",                    /// The team name
            "topLevelId": 0,                          /// ?   
            "contestId": 51084,                       /// The id of the contest
            "categoryId": 83657,                      /// Teams can be within  a category. 
            "totalKm": 182.5,                         /// Number of km done
            "totalKmInsideBoundary": 182.5,           /// Number of km done that fit the contest constraints 
            "kmPerMember": 91.2,
            "kmPerMemberInsideBoundary": 91.2,
            "totalKmProportional": 0,                   /// ?
            "totalKmInsideBoundaryProportional": 0,     /// ? 
            "kmPerMemberProportional": 0,               /// ?
            "kmPerMemberInsideBoundaryProportional": 0, /// ? 
            "captainId": 119679,                        /// user id of the capitain   
            "numberOfMembers": "2",                     /// number of members in the team 
            "numberOfCandidates": "0",                  /// number of ppl on hold until accepted by the capitain
            "logo": "",                                 /// team logo (how to set this) ? 
            "topLevel": null,                           /// ? 
            "areaId": 0,                                /// ?
            "totalKmInsideArea": 182.5,                 /// unlear what is the difference with totalKmInsideBoundary
            "kmInsideAreaPerMember": 91.2 
        },
        ... /// other teams
    },
   "offset": "10",       /// start exporting the team array at a given index
   "limit": "30",        /// maximum of teams to report
   "count": 289,         /// total number of teams, should be the same as the number of entries in the teams array
   "searchKey": ""       /// ?
} 
```

Get per team leaderboard
------------------------
URL: https://www.naviki.org/naviki/api/v5/Contest/leaderboardOfMembersByTeam/{TEAMID}

Where {TEAMID} need to be replaced with the unique id of the team you want to retrieve data from. This id can be obtained by
the "uid" fields in the team object returned by calling the findTeam URL.

Extra parameters are: offset, limit and sort

Example of use:
```console
https://www.naviki.org/naviki/api/v5/Contest/leaderboardOfMembersByTeam/72853
or
https://www.naviki.org/naviki/api/v5/Contest/leaderboardOfMembersByTeam/72853?sort="sortKmInsideBoundary"
```

returns:
```css
{
    "members": [
        {
            "uid": 1654864,                               /// Unique ID of the participant                                 
            "contestId": 51084,                           /// Unique ID of the contest
            "categoryId": 0,                              /// ? 
            "feUserid": 567178,                           /// ? 
            "feUserName": "wheelsoffortune",              /// user name
            "totalKm": 476.6,                             /// total number of km done by this member
            "totalKmInsideBoundary": 476.3,               /// total number of km withing the contest constraint 
            "totalKmProportional": 0,                     /// ? 
            "totalKmInsideBoundaryProportional": 0,       /// ? 
            "topLevel": null,                             /// ?
            "rank": 1                                     /// rank of a member according to the sorting key
        },
        /// ..... 
    ],
    "offset": 0,
    "limit": 20,
    "count": 0,
    "sort": "totalKm"                           /// the sorting key used to rank the data 
}
```

Get the contest leaderboar
--------------------------
URL: https://www.naviki.org/naviki/api/v5/Contest/leaderboardOfMembers/{CONTESTID}

Where {TEAMID} need to be replaced with the unique id of the contest. 
Extra parameters: offset, limit, sort

By default this function is returning a limited amount of members. Extending the limit to a very high value does not change the returned information so in order to get the complete leaderboard it is needed to iterate over the data set. The total number of members is available in the "count" field obtained by calling this url.  

Examples of use:
`console
https://www.naviki.org/naviki/api/v5/Contest/leaderboardOfMembers/51084?offset=0&limit=100
https://www.naviki.org/naviki/api/v5/Contest/leaderboardOfMembers/51084?offset=100&limit=100&sort="totalKmInsideBoundary"
...
`
Each call to the URL returing:
{
    "members": [                                            /// These are the same information as the one obtained from 
        {                                                   
            "uid": 116499,                                  /// Unique contestent ID
            "contestId": 51084,                             /// Unique contest ID
            "categoryId": 0,                                /// ? 
            "feUserid": 515763,                             /// ? 
            "feUserName": "Dede62",                         /// the username
            "totalKm": 1220.5,                              /// number of km done in the contest
            "totalKmInsideBoundary": 1205.3,                /// number of km without the km that does not fit the contest limits.
            "totalKmProportional": 0,                       /// ?
            "totalKmInsideBoundaryProportional": 0,         /// ?
            "topLevel": null,                               /// ? 
            "rank": 1                                       /// ranking of the member according to the sorting key
        },
        ///all the other contestants
     ],
    "offset": 0,
    "limit": 20,
    "count": 4214,             /// Total number of participants, very useful to iterate over 
    "sort": "totalKm"          /// the sorting key
}
