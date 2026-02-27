## Just Another Day on an Airplane

In 2018, the turbulence scared me. It was violent that day, flying over the Pacific Ocean. The plane dropped, hard, and in that moment I realized something obvious that I'd been ignoring: with all the flying I was doing for work, I might die.

Not in some abstract, philosophical sense. Actually die. On a Tuesday. Over the ocean.

If that happened, how would my wife know my passwords? How would she access our accounts? My life and my work are mostly online—secured, encrypted, and invisible. I'd spent years building systems to keep intruders out. I'd never thought about what happens when the person who needs in is the person I love most.

## This Is Something Software Can Solve

I'm an engineer. When I see a problem, I build something.

So I started the first iteration of Swytch: a simple service that would email my wife a link to important documents and passwords if I didn't check in for a while. A digital dead man's switch.

I built the first version in a week, just for myself. It worked fine—until a server died several weeks later. Just bad luck on the hardware. But it made me realize something deeper: I needed a system that could survive servers dying.

Actually, I needed a system that could survive *me* dying.

What if I wanted to send a message to my son years from now? I don't know when I'll die. Neither does the software. The system needs to outlast its creator, remain consistent across failures, and deliver when the time comes—even if that's decades away.

## Software Can't Actually Solve It

I started looking into databases. CockroachDB. CosmosDB. Spanner. All the distributed systems that promised consistency and availability across the globe.

None of them could guarantee what I needed. The CAP theorem loomed over everything: you can have Consistency and Availability, but not during Partition failures. Pick two. Everyone picked two. The best systems offered "eventual consistency"—which, when you're talking about a message to your son after you die, is not good enough.

I needed bounded convergence. I needed to know that when the network heals, the system agrees—quickly, deterministically, without lost writes.

It was 2019. I didn't have a solution, but I had a question that wouldn't let me go.

## The Space Game That Changed Everything

Meanwhile, I started building a game. It was simple-sounding: design a probe, send it out to the stars, watch it explore. But the universe in my game was realistic. The speed of light mattered. Relativistic effects were real.

This meant I had to simulate relativity.

I solved it by hiding relativistic time dilation under latency. The probe's onboard computer was only allowed to run for X amount of time based on its scenario—accelerating, decelerating, approaching a gravity well. The difference between the probe's clock and "real" time was masked as communication delay between probes and FTL relay stations.

It taught me something profound: **latency isn't a bug. Latency is relativity at work.** You can't trade it away. You can't eliminate it. The speed of light is a hard limit, and every distributed system is already operating under relativistic constraints—we just pretend otherwise because Earth is small.

Maybe, instead of fighting latency, you could use it.

I never finished the game. COVID happened. The world turned upside down. But the insight stayed with me.

## PHP Detour

During the pandemic, I got involved in the DAPR project and built the PHP SDK. That led me to DurablePHP—workflows, state machines, durable execution. Complex stuff. I'm still working on marrying that research with FrankenPHP and DAPR. It's coming. Eventually.

But more importantly, FrankenPHP led me to a conference in Lille, France, in 2024.

I was sitting in the keynote, half-listening, thinking about something else entirely. The speaker said something—I don't even remember what exactly—and two dots connected in my brain.

What if we did consensus per-key?

Not per-database. Not per-partition. Per-key. Using something like WPaxos, where leadership could migrate to wherever the writes were happening.

And what if consensus could continue even during a partition? Not pause. Not fail over. Continue.

I spent the rest of the conference day sketching in my notebook. By the end of it, I had the outline of a database that could change CAP modes on demand. A database that could span the world.

Maybe even the solar system.

## The Ballot Problem

I spent the next year implementing different consensus algorithms, trying to make my Lille sketch into reality. Nights. Weekends. The vision was clear; the implementation kept slipping away.

The problem was ballots.

In traditional consensus, ballot numbers determine who's in charge. A new leader takes over, picks a higher ballot number, and continues from where the old leader left off. But if consensus continues during a partition, you get forks. Multiple leaders, multiple ballot sequences, multiple versions of truth.

When the partition heals, which writes win?

In every system I studied, some writes could be lost. The new leader might not know about the old leader's last few operations. This was considered acceptable. "Eventual consistency." "Last write wins."

For me, it was simply unacceptable.

## The Breakthrough

In early 2025, I finally saw it.

The problem wasn't consensus. The problem was treating the log as a single, linear thing that one leader owned completely.

What if ballot numbers applied to *regions* of the log instead? What if leadership was granular—you could be the authority for this range of operations, while someone else was the authority for that range?

But then you need to know which parts of the log are canonical and which forked. So I added dependency tracking: every operation knows what it depends on. If you have the dependencies, you can validate the operation. If you don't, you wait.

And then I realized: if you have dependencies, you don't need clocks.

You don't need synchronized time at all. You don't need snapshots of values. You need *effects*—what happened, and what it depended on.

The Causal Effect Log was born.

## Light Cone Consistency

Once I understood what I'd built, everything simplified.

Instead of consensus, you observe. Instead of agreement, you promise. Every node sees events arrive and can determine, from its own perspective, what's causally consistent and what's still in flight.

I call it Light Cone Consistency—named for the region of spacetime that can causally affect you. My space game taught me that latency is relativity at work. Swytch is what happens when you build a database that finally accepts it.

What that means for distributed computing—and why everything you know about distributed systems might be wrong—is a story for the next post.

