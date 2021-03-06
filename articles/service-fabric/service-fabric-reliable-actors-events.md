---
title: Ereignisse in Actor-basierten Azure-Microservices | Microsoft-Dokumentation
description: "Einführung in Ereignisse für Service Fabric Reliable Actors."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: aa01b0f7-8f88-403a-bfe1-5aba00312c24
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/02/2017
ms.author: amanbha
translationtype: Human Translation
ms.sourcegitcommit: 7033955fa9c18b2fa1a28d488ad5268d598de287
ms.openlocfilehash: 92df9505416c5b4326f007dbf4b920f2c7ec3bd8
ms.lasthandoff: 01/24/2017


---
# <a name="actor-events"></a>Actor-Ereignisse
Actor-Ereignisse bieten eine Möglichkeit zum Senden von Best-Effort-Nachrichten vom Actor an die Clients. Actor-Ereignisse dienen der Actor-Client-Kommunikation und sollten nicht für die Actor-Actor-Kommunikation verwendet werden.

Die folgenden Codeausschnitte zeigen, wie Sie Actor-Ereignisse in Ihrer Anwendung verwenden können.

Definieren Sie eine Schnittstelle, die die vom Actor veröffentlichten Ereignisse beschreibt. Diese Schnittstelle muss von der `IActorEvents` -Schnittstelle abgeleitet werden. Die Argumente der Methoden müssen [Datenvertrag-serialisierbar](service-fabric-reliable-actors-notes-on-actor-type-serialization.md)sein. Die Methoden müssen „void“ zurückgeben, da Ereignisbenachrichtigungen unidirektional und vom Typ „best effort“ sind.

```csharp
public interface IGameEvents : IActorEvents
{
    void GameScoreUpdated(Guid gameId, string currentScore);
}
```

Deklarieren Sie die vom Actor in der Actor-Schnittstelle veröffentlichten Ereignisse.

```csharp
public interface IGameActor : IActor, IActorEventPublisher<IGameEvents>
{
    Task UpdateGameStatus(GameStatus status);

    Task<string> GetGameScore();
}
```

Implementieren Sie auf der Clientseite den Ereignishandler.

```csharp
class GameEventsHandler : IGameEvents
{
    public void GameScoreUpdated(Guid gameId, string currentScore)
    {
        Console.WriteLine(@"Updates: Game: {0}, Score: {1}", gameId, currentScore);
    }
}
```

Erstellen Sie auf dem Client einen Proxy für den Actor, der das Ereignis veröffentlicht, und abonnieren Sie diese Ereignisse.

```csharp
var proxy = ActorProxy.Create<IGameActor>(
                    new ActorId(Guid.Parse(arg)), ApplicationName);

await proxy.SubscribeAsync<IGameEvents>(new GameEventsHandler());
```

Im Failover-Fall kann ein Failover des Actors zu einem anderen Prozess oder Knoten erfolgen. Der Actor-Proxy verwaltet die aktiven Abonnements und verlängert automatisch deren Abonnement. Sie können das Intervall für die Abonnementverlängerung über die API `ActorProxyEventExtensions.SubscribeAsync<TEvent>` steuern. Zum Kündigen des Abonnements verwenden Sie die API `ActorProxyEventExtensions.UnsubscribeAsync<TEvent>` .

Veröffentlichen Sie auf dem Actor die Ereignisse einfach in der Reihenfolge ihres Auftretens. Wenn für das Ereignis Abonnenten vorhanden sind, sendet die Actors-Laufzeit ihnen die Benachrichtigung.

```csharp
var ev = GetEvent<IGameEvents>();
ev.GameScoreUpdated(Id.GetGuidId(), score);
```

## <a name="next-steps"></a>Nächste Schritte
* [Actor-Eintrittsinvarianz](service-fabric-reliable-actors-reentrancy.md)
* [Actor-Diagnose und -Leistungsüberwachung](service-fabric-reliable-actors-diagnostics.md)
* [Actor-API-Referenzdokumentation](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [Beispielcode](https://github.com/Azure/servicefabric-samples)

