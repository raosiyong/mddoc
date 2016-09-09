### Linux wireless driver:ath9k

Tags: 802.11 mac802


***

QCA9331
:    qualcomm atheros 1*1 802.11n WiSoC

QCA9341
:    qualcomm atheros 2*2 300Mbps 802.11n *WiSoC*

>please reserved this document,refer [openwr][openwrturl] 或者使用快捷键 `Ctrl+Shift+M`。
> by &copy; lawrence rao
**Content**

<!-- toc -->

- [Linux wireless driver:ath9k](#linux-wireless-driverath9k)
- [Todo list](#todo-list)
- [mathJax](#mathjaxlatex)
- [highlight source by setting language](#highlight-source-by-setting-language)
- [charts UML Sequence](#charts-uml-sequencehttpsgithubcomknsvmermaid)
- [linux atheros ath9k driver 序列图](#linux-atheros-ath9k-drivermac80211-序列图)
- [waved timing(hw)](#waved-timinghw)
- [Table](#table)

<!-- tocstop -->
**Markdown Enhanced for atom editor** ：
> - linux network framework net/dev/core
> - `linux mac80211 framework`
> - `netlink nl80211/cfg80211 interface`
> - `ath9k ahb in SoC driver`
> - `iw config`
> - `hostapd processing`

>
***

### Todo list
- [ ] only list item
- [ ] 支持以 PDF 格式导出文稿
- [ ] 改进 Cmd 渲染算法，使用局部渲染技术提高渲染效率
- [ ] 新增 Todo 列表功能

### mathJax[^LaTeX]

$$E=mc^2$$

### highlight source by setting language

```cpp
#include <stdio.h>
int main(int argc,char **argv){
  return 0;
}
```


### charts [UML Sequence](https://github.com/knsv/mermaid)

[Plant UML](http://www.plantuml.com "plant UML")

```plantuml
@startuml
start
if (multiprocessor?) then (yes)
  fork
    : Treatment 1;
  fork again
    : Treatment 2;
  end fork
else (monoproc)
  :Treatment 1;
  :Treatment 2;
endif
@enduml
```

```plantuml
@startuml

skinparam roundcorner 20
skinparam maxmessagesize 60
'skinparam handwritten true
skinparam backgroundColor #EEEBDC
skinparam sequence {
    ArrowThickness 1
    ArrowFontColor gray
    ArrowColor black
    'ActorBorderColor DeepSkyBlue
    LifeLineBorderColor blue
    LifeLineBackgroundColor #A9DCDF
    'Participant underline
    ParticipantBorderColor lightGray
    ParticipantBackgroundColor DodgerBlue
    ParticipantFontName Impact
    ParticipantFontSize 17
    ParticipantFontColor #A9DCDF
    ActorBackgroundColor aqua
    ActorFontColor DeepSkyBlue
    ActorFontSize 17
    ActorFontName Aapex
}
Alice -> Bob: Authentication Request
Bob --> Alice: Authentication Response

Alice -> Bob: Another authentication Request
Alice <-- Bob: another authentication Response

@enduml
```
```plantuml
@startuml

'skinparam classAttributeIconSize 0
'skinparam backgroundColor #808080
'skinparam classAttributeFontColor grey
'skinparam stereotypeCBackgroundColor YellowGreen
skinparam class {
''  BackgroundColor lightgrey
''  ArrowColor  lightgrey
''  AttributeFontColor black
''  AttributeFontStyle bold
''  AttributeFontName Courier
''  AttributeFontSize 13
''  FontColor grey
''  BorderColor lightblue  
}
'skinparam monochrome true
class Wiphy{
  +name;
  '..
  char priv[0];
  '--
  {abstract} +register();
  {abstract} +Wiphy_new();
}
'class ieee80211_local << (S,#FF7700) >>{
class ieee80211_local {
  name;
}
Wiphy <|-- ieee80211_local
Wiphy *-- Class04
'Class05 o-- Class06
'Class07 .. Class08
'Class09 -- Class10

@enduml
```

```plantuml
@startuml
' comment
skinparam backgroundColor #808080
skinparam fontColor grey
skinparam activity {
  'FontColor DeepSkyBlue
  'FontSize 17
  ArrowColor light
  FontColor grey
  StartColor Silver
  EndColor Silver
  BarColor Black  
  BackgroundColor lightGray
  'BackgroundColor<< Begin >> Red
  BorderColor #f0f0f0
  FontName Impact
}

(*) -[#0000f0]-> "Initialization"

if "hardware ready" then
  -->[true] "send apdu"
  -->=== S1 ===
  --> "send apmsdu"
  -right-> (*)
else
  ->[false] "rfkill"
  -->[Ending process] (*)
endif

@enduml
```

```mermaid
graph LR;
%%
    D["lady"]
%%    E(("for peace"))
    A((A))
    C>C]
    F[F]

    B-.bond.->D

    C-.->D
    A--note---B
    A-->|arrow|B
    D-->F
    F-->E(("for peace"))

  classDef dotRed fill:#ccf,stroke:#f66,stroke-width:2px,stroke-dasharray: 5, 5;
  classDef fillPink fill:#f96
  %%,stroke:#333,stroke-width:1px;
  class D dotRed
  class E fillPink

```
---
```mermaid
gantt
    title project plan

    section Firstwork
    A task           :a1, 2014-01-01, 30d
    Another task     :after a1  , 20d
    section Another
    Task in sec      :2014-01-12  , 12d
    anther task      : 24d
```


### linux atheros ath9k driver[^mac80211] [序列图]()

```mermaid
sequenceDiagram
    participant ath ahb
    participant ath htc
    participant mac80211
    ath ahb->>+ mac80211: ieee80211_init
    mac80211->>- ath ahb:  tx notifier
    mac80211->>+ ath htc: ieee80211_init
    loop hard_irq
        mac80211->>mac80211: hardware tx irq
    end
    Note right of mac80211: Rational thoughts <br> this is ok

```
---
```mermaid
sequenceDiagram
    Alice->>+John: Hello John, how are you?
    Alice->>+John: John, can yoy hear me?
    John-->>-Alice: Hi Alice, I can hear you!
    John-->>-Alice: I feel great!
    Alice->>Bob: Hello Bob, how are you?
    alt is sick
        Bob->>Alice: Not so good :(
    else is well
        Bob->>Alice: Feeling fresh like a daisy
    end
    opt Extra response
        Bob->>Alice: Thanks for asking
    end
```

### waved timing(hw)
```wavedrom
{signal: [
  {name: '时钟clk', wave: 'p.....|...'},
  {name: 'data', wave: 'x.345x|=.x', data: ['head', 'body', 'tail', 'data']},
  {name: 'req', wave: '010...|1.0'},
  {},
  {name: 'ack', wave: '1.....|01.'}

]}
```

### [Table]()

| 项目        | 价格   |  数量  |
| --------   | -----:  | :----:  |
| 计算机     | \$1600 |   5     |
| 手机        |   \$12   |   12   |
| 管线        |    \$1    |  234  |


<i class="icon-pencil"></i> editor mode


[^LaTeX]: 支持 **LaTeX** 编辑显示支持，例如：$\sum_{i=1}^n a_i=0$， 访问 [MathJax][4] 参考更多使用方法。
[^mac80211]: linux driver layer

[openwrturl]: http://www.openwrt.org
