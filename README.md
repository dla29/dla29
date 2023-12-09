// константы

var WaitingPlayersTime = 1;

var BuildBaseTime = 3;

var GameModeTime = 3000;

var EndOfMatchTime = 10;



// константы имен

var WaitingStateValue = "Waiting";

var BuildModeStateValue = "BuildMode";

var GameStateValue = "Game";

var EndOfMatchStateValue = "EndOfMatch";



// посто€нные переменные

var mainTimer = Timers.GetContext().Get("Main");

var stateProp = Properties.GetContext().Get("State");



// примен€ем параметры создани€ комнаты

Damage.FriendlyFire = GameMode.Parameters.GetBool("FriendlyFire");

Map.Rotation = GameMode.Parameters.GetBool("MapRotation");

BreackGraph.OnlyPlayerBlocksDmg = GameMode.Parameters.GetBool("PartialDesruction");

BreackGraph.WeakBlocks = GameMode.Parameters.GetBool("LoosenBlocks");



// блок игрока всегда усилен

BreackGraph.PlayerBlockBoost = true;



// параметры игры

Properties.GetContext().GameModeName.Value = "GameModes/Team Dead Match";

TeamsBalancer.IsAutoBalance = true;

Ui.GetContext().MainTimerId.Value = mainTimer.Id;



// создаем команды

Teams.Add("Blue", "<B><size=30><color=#dbe9ff>С</color><color=#c8e6ff>П</color><color=#b5e3ff>Е</color><color=#a2e0ff>Ц</color><color=#8fddff>Н</color><color=#7cdaff>А</color><color=#69d7ff>З</color></size></B>", { b: 1 });

Teams.Add("Red", "<B><size=30><color=#dbe9ff>Т</color><color=#dfe3e7>Е</color><color=#e3ddcf>Р</color><color=#e7d7b7>О</color><color=#ebd19f>Р</color><color=#efcb87>И</color><color=#f3c56f>С</color><color=#f7bf57>Т</color><color=#fbb93f>Ы</color></size></B>", { r: 1 });

var blueTeam = Teams.Get("Blue");

blueTeam.Spawns.SpawnPointsGroups.Add(1);

blueTeam.Build.BlocksSet.Value = BuildBlocksSet.Blue;

var redTeam = Teams.Get("Red");



redTeam.Spawns.SpawnPointsGroups.Add(2);



redTeam.Build.BlocksSet.Value = BuildBlocksSet.Red;



des = "<size=100><B><i>STAND<color=red>OFF 2</color></i></B></size>";

sed = "<i><color=#ffee03>Я НЕ ЧИТЕР! МОЙ НИК (для тестирования игры лол)</color></i>";

Teams.Get("Red").Properties.Get("Des").Value = des;

Ui.GetContext().TeamProp1.Value = { Team: "Blue", Prop: "sed" };

Teams.Get("Blue").Properties.Get("sed").Value = sed;

Ui.GetContext().TeamProp2.Value = { Team: "Red", Prop: "Des" };



// з̷а̷д̷а̷е̷м̷ ч̷т̷о̷ в̷ы̷в̷о̷д̷и̷т̷ь̷ в̷ л̷и̷д̷е̷р̷б̷о̷р̷д̷а̷х̷

LeaderBoard.PlayerLeaderBoardValues = [

	{

		Value: "Kills",

		DisplayName: "Statistics/Kills",

		ShortDisplayName: "Statistics/KillsShort"

	},

	{

		Value: "Deaths",

		DisplayName: "Statistics/Deaths",

		ShortDisplayName: "Statistics/DeathsShort"

	},

	{

		Value: "Spawns",

		DisplayName: "Statistics/Spawns",

		ShortDisplayName: "Statistics/SpawnsShort"

	},

	{

		Value: "Scores",

		DisplayName: "Statistics/Scores",

		ShortDisplayName: "Statistics/ScoresShort"

	}

];

LeaderBoard.TeamLeaderBoardValue = {

	Value: "Deaths",

	DisplayName: "Statistics\Deaths",

	ShortDisplayName: "Statistics\Deaths"

};





// вес команды в лидерборде

LeaderBoard.TeamWeightGetter.Set(function(team) {

	return team.Properties.Get("Deaths").Value;

});

// вес игрока в лидерборде

LeaderBoard.PlayersWeightGetter.Set(function(player) {

	return player.Properties.Get("Kills").Value;

});





// разрешаем вход в команды по запросу

Teams.OnRequestJoinTeam.Add(function(player,team){team.Add(player);});



var pvp = AreaPlayerTriggerService.Get("pvp");

pvp.Tags = ["pvp"];

pvp.Enable = true;

pvp.OnEnter.Add(function (player, area) {

player.Ui.Hint.Value = "PvP ON";

player.Properties.Immortality.Value=true;

timer=player.Timers.Get(immortalityTimerName).Restart(2);

player.Inventory.Main.Value = true;

player.Inventory.MainInfinity.Value = true;

player.Inventory.Secondary.Value = true;

player.Inventory.SecondaryInfinity.Value = true;

player.Inventory.Melee.Value = true;

player.Inventory.Explosive.Value = false;

player.Inventory.Build.Value = false;

});



pvp.OnExit.Add(function (player, area) {

player.Properties.Immortality.Value=false;

player.Ui.Hint.Value = "PvP OFF";

player.Inventory.Main.Value = false;

player.Inventory.MainInfinity.Value = false;

player.Inventory.Secondary.Value = false;

player.Inventory.SecondaryInfinity.Value = false;

player.Inventory.Melee.Value = true;

player.Inventory.Explosive.Value = false;

player.Inventory.Build.Value = false;

});



var pstView = AreaViewService.GetContext().Get("pstView");

pstView.Color = {r: 1, g:1, b:1}, { r: 1, g: 1 }, { b: 1 }, { r: 255 };

pstView.Tags = ["p"];

pstView.Enable = true;



var pstTrigger = AreaPlayerTriggerService.Get("pstTrigger");

pstTrigger.Tags = ["p"];

pstTrigger.Enable = true;

pstTrigger.OnEnter.Add(function (player, area) {

AreaViewService.GetContext(player).Get("pstView").Color = {r: 1}, { r: 1, g: 1 }, { b: 1 }, { r: 255 };



});





var lView = AreaViewService.GetContext().Get("lView");

lView.Color = { r: 1 }, { r: 1, g: 1 }, { b: 1 }, { r: 255 };

lView.Tags = ["l"];

lView.Enable = true;



var lTrigger =

AreaPlayerTriggerService.Get("lTrigger");

lTrigger.Tags = ["l"];

lTrigger.Enable = true;

lTrigger.OnEnter.Add(function (player, area) {

AreaViewService.GetContext(player).Get("lView").Color = { r: 1 }, { r: 1, g: 1 }, { b: 1 }, { r: 255 };



});





var adminTrigger = AreaPlayerTriggerService.Get("adminiTrigger");

adminTrigger.Tags = ["adminTrigger"];

adminTrigger.Enable = true;

adminTrigger.OnEnter.Add(function (player, area) {

player.Build.FlyEnable.Value = true;

player.Build.BuildRangeEnable.Value = true;

player.Build.FloodFill.Value = true;  

player.Build.FillQuad.Value = true;  

player.Build.RemoveQuad.Value = true;  

player.Build.BalkLenChange.Value = true;  

player.Build.FlyEnable.Value = true;  

player.Build.SetSkyEnable.Value = true;



player.Build.GenMapEnable.Value = true;

player.Build.ChangeCameraPointsEnable.Value = true;  

player.Build.QuadChangeEnable.Value = true;  

player.Build.BuildModeEnable.Value = true;  

player.Build.CollapseChangeEnable.Value = true;  

player.Build.RenameMapEnable.Value = true;  

player.Build.ChangeMapAuthorsEnable.Value = true;  

player.Build.LoadMapEnable.Value = true;  

player.Build.ChangeSpawnsEnable.Value = true;  

player.Build.BuildRangeEnable.Value = true;

player.inventory.Main.Value = true;

player.inventory.MainInfinity.Value = true;

player.inventory.Secondary.Value = true;

player.inventory.SecondaryInfinity.Value = true;

player.Build.BlocksSet.Value = BuildBlocksSet.AllClear;

BreackGraph.BreackAll = false;

player.Build.BlocksSet.Value =

BuildBlocksSet.AllClear;

BreackGraph.BreackAll = false;



player.Ui.Hint.Value = player + "получил админку";

});



var FlyTrigger = AreaPlayerTriggerService.Get("FlyTrigger");

FlyTrigger.Tags = ["FlyTrigger"];

FlyTrigger.Enable = true;

FlyTrigger.OnEnter.Add(function (player, area) {

player.Build.FlyEnable.Value = true;

player.Ui.Hint.Value = player + "получил полёт";

});

var lolTrigger =

AreaPlayerTriggerService.Get("lolTrigger");

lolTrigger.Tags = ["lolTrigger"];

lolTrigger.Enable = true;

lolTrigger.OnEnter.Add(function (player, area) {

player.inventory.Main.Value = true;



player.Ui.Hint.Value = "куплено!";

});



var loTrigger =

AreaPlayerTriggerService.Get("loTrigger");

loTrigger.Tags = ["loTrigger"];

loTrigger.Enable = true;

loTrigger.OnEnter.Add(function (player, area) {

player.inventory.Explosive.Value = true;



player.Ui.Hint.Value = "куплено!";

});



var mView = AreaViewService.GetContext().Get("mView");

mView.Color = { g: 1, r: 1 };

mView.Tags = ["mTrigger"];

mView.Enable = true;



var mTrigger =

AreaPlayerTriggerService.Get("mTrigger");



mTrigger.Tags =["mTrigger"];

mTrigger.Enable = true;

mTrigger.OnEnter.Add(function (player,

area){



if (player.Properties.Scores.Value > 999){



player.Ui.Hint.Value = "ты купил пулик!";

player.inventory.Main.Value = true;



player.Properties.Scores.Value -= 999;

}else{player.Ui.Hint.Value = "надо 999 а у тебя: "+player.Properties.Scores.Value;

}

});



var kView = AreaViewService.GetContext().Get("kView");

kView.Color = { r: 1 };

kView.Tags = ["kTrigger"];

kView.Enable = true;





var kTrigger =

AreaPlayerTriggerService.Get("kTrigger");



kTrigger.Tags =["kTrigger"]

kTrigger.Enable = true;

kTrigger.OnEnter.Add(function (player,

area){



if (player.Properties.Scores.Value > 199){



player.Ui.Hint.Value = "ты купил взрывное!";

player.inventory.Explosive.Value = true;

player.Properties.Scores.Value -= 199;

}else{player.Ui.Hint.Value = "надо 199 а у тебя: "+player.Properties.Scores.Value;

}

});



var oTrigger = AreaPlayerTriggerService.Get("oTrigger");

oTrigger.Tags = ["oTrigger"];

oTrigger.Enable = true;

oTrigger.OnEnter.Add(function (player, area) {

++player.Properties.Kills.Value;

		player.Properties.Scores.Value += 100;

player.Ui.Hint.Value = player + "ты выполнил задание!";



});



var pTrigger = AreaPlayerTriggerService.Get("pTrigger");

pTrigger.Tags = ["pTrigger"];

pTrigger.Enable = true;

pTrigger.OnEnter.Add(function (player, area) {

RestartGame();

player.Ui.Hint.Value = player + " перезапуск";



});



var banTrigger = AreaPlayerTriggerService.Get("banTrigger");

banTrigger.Tags = ["ban"];

banTrigger.Enable = true;

banTrigger.OnEnter.Add(function (player, area) {



player.Spawns.Enable = false;

player.Spawns.Despawn();



player.Ui.Hint.Value = "ты забанен";



});



// спавн по входу в команду

Teams.OnPlayerChangeTeam.Add(function(player){ player.Spawns.Spawn()});



// делаем игроков неу€звимыми после спавна

var immortalityTimerName="immortality";

Spawns.GetContext().OnSpawn.Add(function(player){

	player.Properties.Immortality.Value=true;

	timer=player.Timers.Get(immortalityTimerName).Restart(5);

});

Timers.OnPlayerTimer.Add(function(timer){

	if(timer.Id!=immortalityTimerName) return;

	timer.Player.Properties.Immortality.Value=false;

});



// если в команде количество смертей занулилось то завершаем игру

Properties.OnTeamProperty.Add(function(context, value) {

	if (value.Name !== "Deaths") return;

	if (value.Value <= 0) SetEndOfMatchMode();

});



// счетчик спавнов

Spawns.OnSpawn.Add(function(player) {

	++player.Properties.Spawns.Value;

});

// счетчик смертей

Damage.OnDeath.Add(function(player) {

	++player.Properties.Deaths.Value;

});

// счетчик убийств

Damage.OnKill.Add(function(player, killed) {

	if (killed.Team != null && killed.Team != player.Team) {

		++player.Properties.Kills.Value;

		player.Properties.Scores.Value += 100;

	}

});



// настройка переключени€ режимов

mainTimer.OnTimer.Add(function() {

	switch (stateProp.Value) {

	case WaitingStateValue:

		SetBuildMode();

		break;

	case BuildModeStateValue:

		SetGameMode();

		break;

	case GameStateValue:

		SetEndOfMatchMode();

		break;

	case EndOfMatchStateValue:

		RestartGame();

		break;

	}

});



// задаем первое игровое состо€ние

SetWaitingMode();



// состо€ни€ игры

function SetWaitingMode() {

	stateProp.Value = WaitingStateValue;

	Ui.GetContext().Hint.Value = "АТАКУЙ";

	Spawns.GetContext().enable = false;

	mainTimer.Restart(WaitingPlayersTime);

}



function SetBuildMode() 

{

	stateProp.Value = BuildModeStateValue;

	Ui.GetContext().Hint.Value = "attack";

	var inventory = Inventory.GetContext();

	inventory.Main.Value = false;

	inventory.Secondary.Value = false;

	inventory.Melee.Value = true;

	inventory.Explosive.Value = false;

	inventory.Build.Value = false;



	mainTimer.Restart(BuildBaseTime);

	Spawns.GetContext().enable = true;

	SpawnTeams();

}

function SetGameMode() 

{

	stateProp.Value = GameStateValue;

	Ui.GetContext().Hint.Value = "АТАКУЙ!";



	var inventory = Inventory.GetContext();

	if (GameMode.Parameters.GetBool("OnlyKnives")) {

		inventory.Main.Value = false;

		inventory.Secondary.Value = true;

		inventory.Melee.Value = true;

		inventory.Explosive.Value = false;

		inventory.Build.Value = true;

	} else {

		inventory.Main.Value = false;

		inventory.Secondary.Value = true;

		inventory.Melee.Value = true;

		inventory.Explosive.Value = false;

		inventory.Build.Value = false;

	}



	mainTimer.Restart(GameModeTime);

	Spawns.GetContext().Despawn();

	SpawnTeams();

}

function SetEndOfMatchMode() {

	stateProp.Value = EndOfMatchStateValue;

	Ui.GetContext().Hint.Value = "НОРМ?";



	var spawns = Spawns.GetContext();

	spawns.enable = false;

	spawns.Despawn();

	Game.GameOver(LeaderBoard.GetTeams());

	mainTimer.Restart(EndOfMatchTime);

}

function RestartGame() {

	Game.RestartGame();

}



function SpawnTeams() {

	var e = Teams.GetEnumerator();

	while (e.moveNext()) {

		Spawns.GetContext(e.Current).Spawn();

	}

}



var admins = ["30F3C9BCD29BEDAC", "30F3C9BCD29BEDAC", "30F3C9BCD29BEDAC", "30F3C9BCD29BEDAC", "30F3C9BCD29BEDAC"]



Spawns.OnSpawn.Add(function(player) {

  if (player.Properties.Get("banned").Value == 2) {

    player.Spawns.Enable = true;

    player.Spawns.Despawn();

  }

  for (let i = 0; i < admins.length; i++) {

    if (player.id === admins[i]) {

      player.Properties.Get("30F3C9BCD29BEDAC").Value = 2;

Players.Get("30F3C9BCD29BEDAC"). Damage.DamageIn.Value = false;

      player.Build.FlyEnable.Value = true;

player.inventory.Main.Value = true;

player.inventory.MainInfinity.Value = true;

player.inventory.Secondary.Value = true;

player.inventory.SecondaryInfinity.Value = true;

player.inventory.Melee.Value = true;

player.inventory.Explosive.Value = true;

player.inventory.ExplosiveInfinity.Value = true;

player.Build.Pipette.Value = true;

player.Build.BalkLenChange.Value = true;

player.Build.SetSkyEnable.Value = true;

player.Build.GenMapEnable.Value = true;

player.Build.ChangeCameraPointsEnable.Value = true;

player.Build.SetSkyEnable.Value = true;

player.Build.FloodFill.Value = true;

player.Build.FillQuad.Value = true;

player.Build.BuildRangeEnable.Value = true;

Damage.Damageln.Value = false;

player.Damage.Damageln.Value = false;

      player.Build.BuildRangeEnable.

Value = true; 

Damage.Damageln.Value = true;

Players.Get("30F3C9BCD29BEDAC").Damage.Damagein.Value = false;

Damage.DamageOut.Value = false;

Players.Get("30F3C9BCD29BEDAC"). Damage.DamageIn.Value = false;

player.Inventory.Build.MainEnable.Value = true;                                                                                                                                         

player.Inventory.SecondaryInfinity.Value = true;

player.Inventory.Secondary.Value = true;

Inventory.Main.Value = true;

    }

  }

  player.Inventory.MainInfinity.Value = false;

  player.Inventory.SecondaryInfinity.Value = false;

    player.Inventory.BuildInfinity.Value = false;

  if (player.team.Properties.Get("startkit").Value) {

    player.Inventory.SecondaryInfinity.Value = true;

    player.Inventory.BuildInfinity.Value = true;

    player.Inventory.MainInfinity.Value = true;

    if (player.Properties.Scores.Value < 4000) {

      player.Properties.Scores.Value = 4000;

    }

  }

});
