# TopTracks

>Timbre<br>
> [ˈtambər] (noun)<br>
>  the character or quality of a musical sound or voice as distinct from its pitch and intensity.
---

## Architecture
Full TypeScript stack built on AWS Serverless.

This is essentially a webapp that allows users to create an account, and
schedule tweets of monthly / 6 month / yearly top tracks. The playlist link is
stored so users can look at a historical record of how their tastes have changed
over time. These playlists are then tweeted out to the user's account.

### Back - End
API processing should be built upon the ExpressJS framework.

#### Requirements
 - Parsing API calls (API wrapper around spotify & twitter APIs?)
   * this has already been written in python
 - Schedule playlist & tweet generation

#### Database
DynamoDB should be used as the database engine for its compatibility with AWS serverless.

UserInfo
| UserId | Oauth Token | Spotify Token | TwitterToken | TweetFrequency | LastTweetDate |
|--------|-------------|---------------|--------------|----------------|---------------|
| 000000 | iamatoken!! | spotifytoken! | twitterToken | Weekly         | Jan 1, 1970   |
| 000001 | iamatoken!! | spotifytoken! | twitterToken | Monthly        | Jan 1, 1970   |
| 000002 | iamatoken!! | spotifytoken! | twitterToken | Annually       | Jan 1, 1970   |

PlaylistsInfo
| PlaylistId | PlaylistUri | CreationDate | UserId |
|------------|-------------|--------------|--------|
| 0          | asdflkjasdf | Jan 1, 1970  | UserId |
| 1          | asdflkjasdf | Jan 1, 1970  | UserId |
| 2          | asdflkjasdf | Jan 1, 1970  | UserId |
| 3          | asdflkjasdf | Jan 1, 1970  | UserId |

```sql
WITH (SELECT * FROM UserInfo WHERE UserId = given_id) AS UserData -- RTE (subquery)

SELECT PlaylistUri
FROM Playlists INNER JOIN UserData
    ON Playlists.UserId = UserData.UserId
WHERE LastTweetDate + TO_OFFSET(TweetFrequency) > DATE()
```

#### Stretch Goals
 - When generating top track playlist, also determine top genres for specified timeframe and link this to the playlist URL
 - Top new tracks prediction (yes i know spotify already does this)

### Front - End
ReactJS
#### Requirements
 - Generate avatars based on uid: https://avatars.dicebear.com/
 - Timeline animation for historical view of all playlists
