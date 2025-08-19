
### Wild Situation for Irritation

```cpp 
CurrentGameMode->GetEnemyPlayers(PlayerState, EnemyActors); // this also make another loop.
for (APlayerController* PlayerController : EnemyActors)  
{  
     ACharacter* Character = Cast<ACharacter>(PlayerController->GetPawn());  
    if (Character)  
    {  
       // Casting Character  
       for (auto& ActiveEffect : EffectManager->ActiveEffects)  
       {  
          if (!IsValid(ActiveEffect))  
          {  
             continue;   
          }  
          if (ActiveEffect->EffectIdentifier == InEffect.GetDefaultObject()->EffectIdentifier)  
          {  
             // Triggered the Active Effect. 
          }  
       }  
    }  
}

```

I shocked how much of irritations make in the single function, it already has 3+ irritations for searching the enemy players, get the player controller for each enemy actors, then searching in active effect of they have and activate the right effect. Plus, the Big O expectation is O(n x m x l) and possibly increase the operation time by liner, in theory. 

It looks like pretty ugly eh? 

### Are that too many Irritations? 

**Yes or No**

In depth of the example, it runs in 6 vs 6 multiplayer game. As the volume of the game, it expected around 6 to 12 in every irritation in the worst case. First irritation has 12, second will be approx. 18 (if each enemy characters has 3 active effects in the worst case) and would be 6 at the last stage of the irritation which searches the enemy who has specific effects. So total size will be roughly **12 x 18 x 6 =  1296,** which is quite ignorable in the modern pc/console environment. 

BUT, if the irritation is persistent or works as the tick, you have to cautious making the irritations, because it would explode how much irritation calculation happens in one second. As the example at the beginning, even each irritations have single digit size, but it would exploded enormous number of irritation in few seconds. (38,880 total in 30 seconds and 388,800 in 5 minutes, plus the network delay, extra..)

TLDR; it would depend on how much and how long irritation function happens. 

### But Network Delay Matters

[^1]However, in the multiplayer game, network delay impacts the performance then irritation logic. Network delay refers to the delay between a player's action and corresponding response in an online game. it occurs due to the time it makes for data packets to travel from player device to the game server and back. High latency can significantly impact in the gaming experience. 

In the Unreal editor, you can simulate the minimum latency and maximum latency situation to check how gameplay performance works in the real time situation. 

![[network-emulation.png]]
Using network emulation and simulate the game by each network performance. 

Also, you can check DotNet network profiler in the binary folder of the project to check the network performance profile to check packet lost and network delay with the real time data. 
https://www.youtube.com/watch?v=vWjEbfaNHfs

[^1]: https://www.tencentcloud.com/techpedia/112179
