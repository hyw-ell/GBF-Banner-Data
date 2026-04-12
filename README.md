# GBF-Banner-Data
Stores banner data for Granblue Fantasy. Banners are categorized by the year and month in which they started. Banner data is logged using [GBF-Banner-Logger](https://github.com/hyw-ell/GBF-Banner-Logger).

## Schema
> [!NOTE]
> - Banner Data JSON files contain an object with a bannerInfo (object) property and an items (array) property.
> - Properties containing `rate1` in their name refer to draw rates for single draws or the first 9 draws of a 10-Part Draw.
> - Properties containing `rate2` in their name refer to draw rates for the last draw of a 10-Part Draw (which excludes R items and guarantees an SR or better).

### bannerInfo
| Property                   | Type     | Description                                                                                                                                                                                                                                                                                                                 |
|:---------------------------|:---------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id                         | string   | 6-digit numeric ID for the banner.                                                                                                                                                                                                                                                                                          |
| key                        | string   | 8-digit alphanumeric key which can be used to find the banner background, info text, and some promotional images. :question:                                                                                                                                                                                                |
| start <br> end             | string   | The time at which the banner starts or ends in Japan Standard Time.                                                                                                                                                                                                                                                         |
| featuredItemIDs            | string[] | List of IDs for items that are featured. Note: this only includes items with images appearing on the [draw page](https://game.granbluefantasy.jp/#gacha), and may not include everything on the [included items page](https://game.granbluefantasy.jp/#gacha/selected). Summons unfortunately usually aren't included here. |
| seasons                    | string[] | The season(s) (Valentine, Summer, Yukata, Halloween, Holiday) for characters included on this banner. Does not include Grand Zooey's season.                                                                                                                                                                                |
| series                     | string[] | The series (Valentine, Summer, Yukata, Halloween, Holiday, Grand, Fantasy, 12 Generals) for characters included on this banner. Does not include Grand Zooey's season.                                                                                                                                                      |
| totalRate1 <br> totalRate2 | number   | The total combined draw rate of all items in the first or second group of items.                                                                                                                                                                                                                                            |
| drawRates                  | object   | Contains the rates for each rarity. Format: <pre lang="yaml">{SS Rare: string, S Rare: string, Rare: string}</pre>                                                                                                                                                                                                          |

#### :question: Banner images which rely on the banner key:
- Background: [https://prd-game-a-granbluefantasy.akamaized.net/assets_en/img/sp/gacha/`key`/header/bg.jpg](https://prd-game-a-granbluefantasy.akamaized.net/assets_en/img/sp/gacha/8nkt9t7y/header/bg.jpg)
- Banner Info Text: [https://prd-game-a-granbluefantasy.akamaized.net/assets_en/img/sp/gacha/`key`/header/logo.png](https://prd-game-a-granbluefantasy.akamaized.net/assets_en/img/sp/gacha/8nkt9t7y/header/logo.png)
    - For banners [like these](https://game.granbluefantasy.jp/#news/detail/7087/2/1/1) which feature a different element on rate up each day, the banner key will stay the same between all 6 banners, but _`element` needs to be added to the link after **logo** and before **.png** to get the info text for each element's banner.
    - Example: https://prd-game-a-granbluefantasy.akamaized.net/assets_en/img/sp/gacha/1a2ro6cj/header/logo_fire.png
- Fate Episodes promotional image for featured characters: [https://prd-game-a1-granbluefantasy.akamaized.net/assets_en/img/sp/gacha/`key`/feature/index/`id + 1`_`key`.jpg](https://prd-game-a1-granbluefantasy.akamaized.net/assets_en/img/sp/gacha/8nkt9t7y/feature/index/206881_8nkt9t7y.jpg)
    - `id + 1` is the banner ID + 1 (e.g. if banner ID is 100000, the value here should be 100001)
    - Note that there is an underscore between the `id + 1` and the `key`.
    - This image is infrequently updated and can show characters that were featured in the past and not featured on the current banner
- Featured summon images: [https://prd-game-a1-granbluefantasy.akamaized.net/assets_en/img/sp/gacha/`key`/feature/index/`id`.jpg](https://prd-game-a1-granbluefantasy.akamaized.net/assets_en/img/sp/gacha/8nkt9t7y/feature/index/2040423000.jpg)
    - `id` is the summon's ID. Does not work with items that are not summons.

### directory
The directory is an array of bannerInfo objects ONLY, to make it easier to find a banner. These bannerInfo objects include an additional `path` (string) property which provides the filepath to the banner data file.

### items
| Property                 | Type                     | Description                                                                                                                                                            |
|:-------------------------|:-------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| name                     | string                   | The name of the weapon or summon.                                                                                                                                      |
| id                       | string                   | The ID of the weapon or summon.                                                                                                                                        |
| rarity                   | string                   | The rarity of the weapon or summon.                                                                                                                                    |
| element                  | string                   | The element of the weapon or summon.                                                                                                                                   |
| type                     | string                   | The type of the weapon (sabre, dagger, etc). If the item is a summon, the value will be "summon".                                                                      |
| rate1 <br> rate2         | number                   | The draw rate for the item in the first or second group of items.                                                                                                      |
| cum_rate1 <br> cum_rate2 | number                   | The cumulative draw rate for the item in the first or second group of items. This property is included to make it easy to use a random number generator to draw items. |
| rate_up                  | boolean                  | Whether the item is on rate up.                                                                                                                                        |
| character                | string&nbsp;\|&nbsp;null | The character associated with the weapon. If the item does not come with a character or is a summon, the value will be null.                                           |

### characters
| Property    | Type     | Description                                                                                                                                                                                                               |
|:----------- |:-------- |:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| number      | string   | The character's number, which denotes the order in which they were released (only for characters that can appear in the draw pool). Example: Yuni (Holiday) was the 650th character to be released, so she is number 650. |
| id          | string   | The character's id.                                                                                                                                                                                                       |
| name        | string   | The character's name.                                                                                                                                                                                                     |
| short_name  | string   | The character's short name. Example: Dorothy and Claudia's short name is "The Maids".                                                                                                                                     |
| style       | string   | The character's style (Balanced, Attack, Defense, Heal, Special).                                                                                                                                                         |
| rarity      | string   | The character's rarity (SS Rare, S Rare, Rare).                                                                                                                                                                           |
| element     | string   | The character's element.                                                                                                                                                                                                  |
| uncaps      | number   | The number of times the character can be uncapped.                                                                                                                                                                        |
| specialties | string[] | The character's specialties.                                                                                                                                                                                              |
| series      | string[] | The series (Valentine, Summer, Yukata, Halloween, Holiday, Grand, Fantasy, 12 Generals) for the character.                                                                                                                |
| races       | string[] | The races for the character. There can be multiple because 2-in-1 units exist.                                                                                                                                            |
| voice_actor | string   | The character's voice actor.                                                                                                                                                                                              |
| add_date    | string   | The date on which the character was added.                                                                                                                                                                                |
| weapon_id   | string   | The ID for the weapon associated with the character.   