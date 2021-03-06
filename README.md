# Espionage

[![Build status](https://ci.appveyor.com/api/projects/status/qf56bjnwc1ywvu31/branch/master?svg=true)](https://ci.appveyor.com/project/mattleibow/espionage/branch/master)  [![NuGet](https://img.shields.io/nuget/dt/Espionage.svg)](https://www.nuget.org/packages/Espionage)  [![NuGet Pre Release](https://img.shields.io/nuget/vpre/Espionage.svg)](https://www.nuget.org/packages/Espionage)

A developer-friendly library for getting started in the world of espionage and clandestine operations.

## Getting Started

Like with all .NET libraries, the first we need to add is the using statement:

```csharp
using Espionage;
```

Once that is done, we can start the intelligence gathering. Because we are working in
some world, whether real or imaginary, we want to prepare the world for our secret
agents:

```csharp
// we want the default world
World.LoadDefaultWorld();

// next, we want to get a grip on this new world
var realWorld = World.Current;
```

Now that we have a world, we want to set up our HQ and, unfortunately for our world,
the enemy HQ as well:

```csharp
var goodGuys = realWorld.Headquarters;
var badGuys = realWorld.EnemyHeadquarters;
```

As soon as the enemies have a base, they build facilities to create weapons to
destroy us. Each facility has several documents that contain top-secret information
that we can use to take them down. In order to get hold of these documents, we will
need to train some secret agents:

```csharp
// train the agents' handler
var handler = await goodGuys.TrainOperativeAsync();

// now, train the agent
var spy = await handler.TrainAgentAsync();
```

We can train as many operatives and agents as we need, but as soon as we have an
agent, we can assign him to a facility:

```csharp
// locate a facility with documents of interest
var facility = badGuys.Facilities.FirstOrDefault(f => f.Documents.Any());

// then, we can assign the agent
var infiltrated = await spy.InfiltrateAsync(facility);

// if the agent was unable to gain the trust, try again
while (!infiltrated)
{
    infiltrated = await spy.InfiltrateAsync(facility);
}
```

As soon as the agent is embedded in the enemy facility, he can start looking around
for the documents:

```csharp
// try grab a document
var doc = await spy.RetrieveDocumentAsync();

// if the document was secured, try a different method
while (doc == null)
{
    doc = await spy.RetrieveDocumentAsync();
}
```

Now that we have the document in hand, we can give it to our headquarters, but we
need to remember that only agents can infiltrate a facility, and only the operative
can send the documents back to HQ. The agent has to hand off the documents to the
operative first, without getting caught:

```csharp
// arrange a meeting to exchange the documents
var handoff = await spy.DeliverDocumentsAsync();

// if the handoff was too risky, try again at a later stage
while (!handoff)
{
    handoff = await spy.DeliverDocumentsAsync();
}
```

Now that the operative has the documents, the agent can go back to being
invisible. The operative, however, has to get the documents back to HQ:

```csharp
// send the documents back to base
var delivered = await handler.DeliverDocumentsAsync();

// if the documents couldn't be sent without being discovered, send later
while (!delivered)
{
    delivered = await handler.DeliverDocumentsAsync();
}
```

At last! Some of the documents have been delivered to our HQ and they can start
creating defenses against those horrible weapons. There may be more documents
at that facility - and quite possibly at other facilities. HQ can create more
operatives and/or agents, send them out into the field and retrieve all the
documents.