# Anime Relations

This repository includes anime relation data for [Taiga](https://github.com/erengy/taiga). It is used to redirect an episode to another, which is required to handle special episodes and the case where fansub groups use continuous numbering scheme in their releases.

## Rule syntax

    (\d+)|(\d+):(\d+)(?:-(\d+|\?))? -> (\d+)|(\d+):(\d+)(?:-(\d+|\?))?(!)?
    └─┬─┘ └─┬─┘ └────────┬────────┘    └─┬─┘ └─┬─┘ └────────┬────────┘└┬─┘
      1     2            3               1     2            3          4

1. MyAnimeList ID - `https://myanimelist.net/anime/{id}/{title}`
2. Kitsu ID - `https://kitsu.io/api/edge/anime?filter[text]={title}`
3. Episode number or range (`?` is used for unknown episode count)
4. Appending `!` to a rule is a shorthand for creating a new rule where destination ID is redirected to itself

## Example

The first season of *Fate/Zero* has 13 episodes, yet it is possible to encounter filenames that go beyond this number: 

    [Coalgirls]_Fate_Zero_14_(1280x720_Blu-ray_FLAC)_[E56A8415].mkv

To handle this case, we create the following rule:

    # Fate/Zero -> ~ 2nd Season
    - 10087|6028:14-25 -> 11741|7658:1-12!

Here we declare that 14th to 25th episodes of *Fate/Zero* are to be identified as the 1st to 12th episodes of *Fate/Zero 2nd Season*:

<table>
  <tbody>
    <tr>
      <td align="right"><strong>S1</strong></td>
      <td align="right">14</td>
      <td align="right">15</td>
      <td align="right">16</td>
      <td align="right">17</td>
      <td align="right">18</td>
      <td align="right">19</td>
      <td align="right">20</td>
      <td align="right">21</td>
      <td align="right">22</td>
      <td align="right">23</td>
      <td align="right">24</td>
      <td align="right">25</td>
    </tr>
    <tr>
      <td align="right"><strong>S2</strong></td>
      <td align="right">1</td>
      <td align="right">2</td>
      <td align="right">3</td>
      <td align="right">4</td>
      <td align="right">5</td>
      <td align="right">6</td>
      <td align="right">7</td>
      <td align="right">8</td>
      <td align="right">9</td>
      <td align="right">10</td>
      <td align="right">11</td>
      <td align="right">12</td>
    </tr>
  </tbody>
</table>

By appending an `!` to the rule, we also handled the cases such as `Fate Zero S2 - 14`. This basically creates another rule:

    # Fate/Zero 2nd Season -> ~
    - 11741|7658:14-25 -> 11741|7658:1-12

## How to contribute

1. Look up MyAnimeList and Kitsu IDs of both anime.
2. Create a new rule and place it in alphabetical order, using the main title from MyAnimeList.
3. Update the `last_modified` date in `YYYY-MM-DD` format.
4. Make sure the rule is working, by testing it before sending a pull request.
5. In the pull request, indicate which fansub groups' releases require the new rule.

## License

This repository is in the public domain.
