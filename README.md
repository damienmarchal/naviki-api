# naviki-api
Naviki is a cycling application used by challenges organizer to record participants gps traces. Despite not documented naviki seems to have an open API to access the challenges data. To help anyone to develop custom tools or widgets to animate the challenge here is a brief documentation of the API as it is in 20 June 2020. 

Contest informations
------------------------
URL: https://www.naviki.org/naviki/api/v5/Contest/2/findContest/CONTESTID

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
URL: https://www.naviki.org/naviki/api/v5/Contest/findTeams/CONTESTID

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
========================

Get the general leaderboars 
===========================
