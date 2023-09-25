# 语音(利用alt:V’s内置语音)

alt:V默认有内置的语音功能，这就是你如何使用它。

这种声音可以用于3D或2D语音解决方案，以实现以下功能:

-   3D和2D语音聊天
-   电话通话
-   无线电通信
-   更多

**下面的代码片段将向您展示如何使用javascript类为角色扮演服务器实现3D语音**

## 函数解释

快速解释什么语音功能是可用的alt:V API

**玩家属性(客户端)**

`isTalking` boolean - 如果该玩家在说话，则为true

`micLevel` number - 玩家说话音量

```js
import * as alt from 'alt-client';

let localPlayer = alt.Player.local;
alt.log('Is player talking: ' + localPlayer.isTalking);
alt.log('Player talking volume: ' + localPlayer.micLevel);
```

**VoiceChannel Class**

`new VoiceChannel(isSpatial: boolean, maxDistance: number)` constructor - 空间false是指全局语音信道，空间true是指以最大距离为范围的三维语音信道

`addPlayer(player: alt.Player)` function - 将玩家添加到特定频道

`removePlayer(player: alt.Player)` function - 将玩家移动到特定频道

`mutePlayer(player: alt.Player)` function - 使特定通道内的玩家静音

`unmutePlayer(player: alt.Player)` function - 取消特定频道内的播放器静音

`isPlayerInChannel(player: alt.Player)` function - 检查玩家是否已经在特定频道中

`isPlayerMuted(player: alt.Player)` function - 检查玩家在特定频道是否静音

```js
import * as alt from 'alt-server';

let testChannel = new alt.VoiceChannel(true, 20);
alt.on('playerConnect', player => {
    testChannel.addPlayer(player);
    testChannel.unmutePlayer(player);
});

alt.on('playerDisconnect', player => {
    testChannel.removePlayer(player);
});

alt.on('playerDeath', player => {
    //在testchannel中静音播放器，如果已死亡并连接到语音
    if (testChannel.isPlayerInChannel(player) && !testChannel.isPlayerMuted(player)) {
        testChannel.mutePlayer(player);
    }
});
//check example
```

## Example Usage 3D (Serverside)

_Serverside_

```js
import * as alt from 'alt-server';

class AltvVoiceServerModule {
    constructor() {
        //yell channel
        this.longRangeChannel = new alt.VoiceChannel(true, 16);
        //speak channel
        this.midRangeChannel = new alt.VoiceChannel(true, 8);
        //whisper channel
        this.lowRangeChannel = new alt.VoiceChannel(true, 3);
        this.registerEvents();
        alt.log('AltvVoiceServerModule init');
    }

    registerEvents() {
        //或者在玩家认证后成功生成后调用它
        alt.on('playerConnect', player => {
            this.addToVoiceChannels(player);
            this.changeVoiceRange(player);
        });
        //处理玩家断开连接
        alt.on('playerDisconnect', this.removePlayerFromChannels.bind(this));
        //处理玩家游戏崩溃/实体失效
        alt.on('removeEntity', this.removePlayerFromChannels.bind(this));
        //处理玩家语音变化
        alt.onClient('server:ChangeVoiceRange', this.changeVoiceRange.bind(this));
    }

    /**
     * 清除给定玩家的通道
     * @param  {alt.Player} player
     * @returns {null}
     */
    removePlayerFromChannels(player) {
        if (this.lowRangeChannel.isPlayerInChannel(player)) {
            this.lowRangeChannel.removePlayer(player);
        }
        if (this.midRangeChannel.isPlayerInChannel(player)) {
            this.midRangeChannel.removePlayer(player);
        }
        if (this.longRangeChannel.isPlayerInChannel(player)) {
            this.longRangeChannel.removePlayer(player);
        }
    }

    /**
     * 添加播放器到所有语音频道
     * @param  {alt.Player} player
     * @returns {null}
     */
    addToVoiceChannels(player) {
        this.lowRangeChannel.addPlayer(player);
        this.midRangeChannel.addPlayer(player);
        this.longRangeChannel.addPlayer(player);
    }

    /**
     * 静音播放器在所有语音频道
     * @param  {alt.Player} player
     * @returns {null}
     */
    muteInAllChannels(player) {
        this.lowRangeChannel.mutePlayer(player);
        this.midRangeChannel.mutePlayer(player);
        this.longRangeChannel.mutePlayer(player);
    }

    /**
     * 针对玩家指定范围，在所有不能听到他的频道中将其静音。
     * @param  {alt.Player} player
     * @param  {number} range
     * @returns {null}
     */
    muteNotInRangeChannels(player, range) {
        switch (range) {
            case 3:
                this.midRangeChannel.mutePlayer(player);
                this.longRangeChannel.mutePlayer(player);
                break;
            case 8:
                this.lowRangeChannel.mutePlayer(player);
                this.longRangeChannel.mutePlayer(player);
                break;
            case 15:
                this.lowRangeChannel.mutePlayer(player);
                this.midRangeChannel.mutePlayer(player);
                break;
            default:
                break;
        }
    }

    /**
     * 更改给定玩家的语音范围并在新范围的频道中取消静音。
     * @param  {alt.Player} player
     * @returns {null}
     */
    changeVoiceRange(player) {
        if (!player.voiceRange) {
            player.voiceRange = 0;
        }
        switch (player.voiceRange) {
            case 0:
                player.voiceRange = 3;
                this.muteNotInRangeChannels(player, 3);
                this.lowRangeChannel.unmutePlayer(player);
                alt.emitClient(player, 'client:UpdateCurrentAltVoiceRange', 3);
                break;
            case 3:
                player.voiceRange = 8;
                this.muteNotInRangeChannels(player, 8);
                this.midRangeChannel.unmutePlayer(player);
                alt.emitClient(player, 'client:UpdateCurrentAltVoiceRange', 8);
                break;
            case 8:
                player.voiceRange = 15;
                this.muteNotInRangeChannels(player, 15);
                this.longRangeChannel.unmutePlayer(player);
                alt.emitClient(player, 'client:UpdateCurrentAltVoiceRange', 15);
                break;
            case 15:
                player.voiceRange = 0;
                this.muteInAllChannels(player);
                alt.emitClient(player, 'client:UpdateCurrentAltVoiceRange', 0);
                break;
            default:
                break;
        }
    }
}

//初始化语音类实例
export const AltvVoiceServerModuleInstance = new AltvVoiceServerModule();
```

## Example Usage 3D (Clientside)

_Clientside_

```js
import * as alt from 'alt-client';

class AltvVoiceClientModule {
    constructor() {
        //localPlayer对象的快捷方式
        this.localPlayer = alt.Player.local;
        //检查状态变化的时间间隔
        this.interval = null;
        //缓存通话状态
        this.talkingState = false;
        //缓存语音范围
        this.currentRange = 0;
        this.registerEvents();
        alt.log('AltvVoiceClientModule init');
    }

    registerEvents() {
        alt.on('keydown', key => {
            if (key == 107) {
                //按下Num+来改变你被其他播放器听到的音域
                alt.emitServer('server:ChangeVoiceRange');
            }
        });

        alt.onServer('client:UpdateCurrentAltVoiceRange', range => {
            this.currentRange = range;
            //emit new range to your user interface
            /* example payload
                {
                muted: range === 0 ? true : false,
                range: range
                }
            */
        });

        this.registerTalkingInterval();
    }

    /*
     * 处理通话状态变化的间隔时间
     * i.e 显示在你的UI，如果这个玩家正在说话(像ts3语音led)
     */
    registerTalkingInterval() {
        this.interval = alt.setInterval(() => {
            //仅当状态改变时触发 
            if (this.talkingState !== this.localPlayer.isTalking && this.currentRange !== 0) {
                this.talkingState = this.localPlayer.isTalking;
                //将对话状态更改到ui {this.talkingState}
            }
            if (this.talkingState && this.currentRange === 0) {
                //将对话状态更改为UI {false}
            }
        }, 444);
    }
}

//初始化语音类实例
export const AltvVoiceClientModuleInstance = new AltvVoiceClientModule();
```