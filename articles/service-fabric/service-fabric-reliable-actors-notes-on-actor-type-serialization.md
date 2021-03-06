---
title: Hinweise zur Serialisierung des Actor-Typs in Reliable Actors
description: Erörtert grundlegende Anforderungen für das Definieren serialisierbarer Klassen, die zum Definieren von Statusangaben und Schnittstellen für Service Fabric Reliable Actors verwendet werden können.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: ''

ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/06/2015
ms.author: vturecek

---
# Hinweise zur Typserialisierung von Service Fabric Reliable Actors
Die Argumente aller Methoden, die Ergebnistypen der von jeder Methode zurückgegebenen Aufgaben in einer Actor-Schnittstelle sowie Objekte, die im Zustands-Manager des Actors gespeichert sind, müssen [mit DataContract serialisiert werden können](https://msdn.microsoft.com/library/ms731923.aspx). Dies gilt auch für die Argumente der Methoden, die in [Actor-Ereignisschnittstellen](service-fabric-reliable-actors-events.md#actor-events) definiert werden. (In Actor-Ereignisschnittstellen definierte Methoden geben immer „void“ zurück).

## Benutzerdefinierte Datentypen
In diesem Beispiel definiert die folgende Actor-Schnittstelle eine Methode, die einen benutzerdefinierten Datentyp namens `VoicemailBox` zurückgibt.

```csharp
public interface IVoiceMailBoxActor : IActor
{
    Task<VoicemailBox> GetMailBoxAsync();
}
```

Die Schnittstelle wird durch einen Actor implementiert, der den Zustands-Manager zum Speichern eines `VoicemailBox`-Objekts verwendet:

```csharp
[StatePersistence(StatePersistence.Persisted)]
public class VoiceMailBoxActor : Actor, IVoicemailBoxActor
{
    public Task<VoicemailBox> GetMailboxAsync()
    {
        return this.StateManager.GetStateAsync<VoicemailBox>("Mailbox");
    }
}

```

In diesem Beispiel wird das `VoicemailBox`-Objekt serialisiert, wenn:

* Das Objekt zwischen einer Actor-Instanz und einem Anrufer übertragen wird.
* Das Objekt im Zustands-Manager gespeichert wird, von wo aus es dauerhaft auf einem Datenträger gespeichert und auf die anderen Knoten repliziert wird.

Das Reliable Actor-Framework verwendet DataContract-Serialisierung. Aus diesem Grund müssen die benutzerdefinierten Objekte und deren Member mit den Anmerkungen **DataContract** beziehungsweise **DataMember** versehen sein.

```csharp
[DataContract]
public class Voicemail
{
    [DataMember]
    public Guid Id { get; set; }

    [DataMember]
    public string Message { get; set; }

    [DataMember]
    public DateTime ReceivedAt { get; set; }
}
```

```csharp
[DataContract]
public class VoicemailBox
{
    public VoicemailBox()
    {
        this.MessageList = new List<Voicemail>();
    }

    [DataMember]
    public List<Voicemail> MessageList { get; set; }

    [DataMember]
    public string Greeting { get; set; }
}
```

## Nächste Schritte
* [Actor-Lebenszyklus und Garbage Collection](service-fabric-reliable-actors-lifecycle.md)
* [Actor-Timer und -Erinnerungen](service-fabric-reliable-actors-timers-reminders.md)
* [Actor-Ereignisse](service-fabric-reliable-actors-events.md)
* [Actor-Eintrittsinvarianz](service-fabric-reliable-actors-reentrancy.md)
* [Actor-Polymorphie und objektorientierte Entwurfsmuster](service-fabric-reliable-actors-polymorphism.md)
* [Actor-Diagnose und -Leistungsüberwachung](service-fabric-reliable-actors-diagnostics.md)

<!---HONumber=AcomDC_0713_2016-->