# Director mode

`中介者模式的作用是解决对象与对象之间的紧耦合关系，所有相关的对象都通过中介者对象来通信`

### 中介者模式 泡泡堂游戏
```javascript
// 玩家数目为2，当一名玩家死亡，另一名玩家就获胜
var Player = function(name) {
    this.name = name;
    this.enemy = null;
}
Player.prototype.win = function() {
    console.log(this.name, 'won');
}
Player.prototype.lose = function() {
    console.log(this.name, 'lose');
}
Player.prototype.die = function() {
    this.lose();
    this.enemy.win();
}
var player1 = new Player('zs');
var player2 = new Player('ls');
player1.enemy = player2;
player2.enemy = player1;

player2.die();
```

### 当人数增加，并且分两队时
```javascript
// 当2个人的时候这样做是没有问题的，但是当有8个人，分两队， 这样实现方式很低效。
var players = [];
function Player(name, teamColor) {
    this.partners = []; // 队友
    this.enemies = [];
    this.state = 'live';
    this.name = name;
    this.teamColor = teamColor;
}

// 成功，失败
Player.prototype.win = function() {
    console.log('winner' + this.name);
}
Player.prototype.lose = function() {
    console.log('lose' + this.name);
}
// 失败，遍历队友，如果全失败则输。
Player.prototype.die = function() {
    var all_dead = true;
    this.state = 'dead';
    for(var i=0, partner;partner=this.partners[i++];) {
        if (partner[i].state === 'live') {
            all_dead = false;
            break;
        } 
    }

    if (all_dead) {
        this.lose();
        for(var i=0,partner;partner=this.partners[++i];) {
            partner.lose();
        }

        // 遍历敌人win
        //....
    }
}

// 最后定义一个工厂函数来创建玩家
var playerFactory = function(name, teamColor) {
    var newPlayer = new Player(name, teamColor);
    for(var i=0,player; player=players[++i];) {
        if(player.teamColor === newPlayer.teamColor) {
           player.partners.push(newPlayer);
           newPlayer.partners.push(player); 
        } else {
            player.enemies.push(newPlayer);
            newPlayer.enemies.push(player);
        }
        players.push(newPlayer);
    }
    return newPlayer;
}
```

### 当人数继续增加，并且每个对象的状态发生变化时， 比如移动，必须循环通知其他对象。如果有一个玩家掉线，必须从其他玩家的队友列表和敌人列表中删除这个玩家。这个时候就需要使用中介者模式重构代码。其中中介者就是一个对象，它负责对象之间耦合的逻辑。比如上例中player对象中应该只需要它本省的属性，partner,enemy, 等都应该在中介者对象中处理。
```javascript
function Player (name, teamColor) {
    this.name = name;
    this.teamColor = teamColor;
    this.state = 'live';
}
Player.prototype.win //...
Player.prototype.lose   //...

Player.prototype.die = function() {
    this.state = 'dead';
    // 这里为了避免对象之间耦合, 所以交给中介者处理
    playerDirector.ReceiveMessage('playerDead', this);
}
Player.prototype.remove = function() {
    playerDirector.ReceiveMessage('removePlayer', this);
}
Player.prototype.changeTeam = function(color) {
    playerDirector.ReceiveMessage('changeTeam', this, color);
}

//  改写工厂对象
var playerFactory = function(name, teamColor) {
    var newPlayer = new Player(name, teamColor);
    playerDirector.ReceiveMessage('addPlayer', newPlayer);
    return newPlayer;
}

// 实现中介者对象有两种方式：
    // 1.利用发布-订阅模式 playerDirector 为订阅者, player 为发布者
    // 2.playerDirector开发一些接口。

var playerDirector = (function() {
    var players = {},
    operations = {};

    operations.addPlayer = function(player) {
        var teamColor = player.teamColor;
        players[teamColor] = players[teamColor] || [];
        players[teamColor].push(player);
    };

    operations.removePlayer = function(player) {
        var teamColor = player.teamColor,
            teamPlayers = players[teamColor] || [];
        //遍历删除
    };

    operations.changeTeam = function(player, newTeamColor) {
        operations.removePlayer(player);
        player.teamColor = newTeamColor;
        operations.addPlayer(player);

    }

    operations.playerDead = function(player) {
        //  same as last example
    }

    var ReceiveMessage = function() {
        var message = Array.prototype.shift.call(arguments);
        operations[message].apply(this, arguments);
    }

    return {
        ReceiveMessage: ReceiveMessage,
    }
})();
```