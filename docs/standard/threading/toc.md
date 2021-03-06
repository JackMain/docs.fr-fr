# [Thread](index.md)
## [Éléments fondamentaux du threading managé](managed-threading-basics.md)
### [Threads et threading](threads-and-threading.md)
### [Exceptions dans les threads managés](exceptions-in-managed-threads.md)
### [Synchronisation des données pour le multithreading](synchronizing-data-for-multithreading.md)
### [États des threads managés](managed-thread-states.md)
### [Threads de premier plan et d'arrière-plan](foreground-and-background-threads.md)
### [Threading managé et non managé dans Windows](managed-and-unmanaged-threading-in-windows.md)
### [Thread.Suspend, Garbage Collection et les points sans risque](thread-suspend-garbage-collection-and-safe-points.md)
### [Stockage local des threads : champs statiques et emplacements de données relatifs à un thread](thread-local-storage-thread-relative-static-fields-and-data-slots.md)
### [Annulation dans les threads managés](cancellation-in-managed-threads.md)
#### [Guide pratique pour écouter les demandes d’annulation par l’interrogation](how-to-listen-for-cancellation-requests-by-polling.md)
#### [Guide pratique pour enregistrer des rappels pour les demandes d’annulation](how-to-register-callbacks-for-cancellation-requests.md)
#### [Guide pratique pour écouter les demandes d’annulation avec des handles d’attente](how-to-listen-for-cancellation-requests-that-have-wait-handles.md)
#### [Guide pratique pour écouter plusieurs demandes d’annulation](how-to-listen-for-multiple-cancellation-requests.md)
## [Utilisation des threads et du threading](using-threads-and-threading.md)
### [Création de threads et passage de données au démarrage](creating-threads-and-passing-data-at-start-time.md)
### [Suspension et reprise des threads](pausing-and-resuming-threads.md)
### [Détruire de threads](destroying-threads.md)
### [Planification de threads](scheduling-threads.md)
### [Annulation de threads de façon coopérative](canceling-threads-cooperatively.md)
## [Bonnes pratiques de threading géré](managed-threading-best-practices.md)
## [Fonctionnalités et objets de threading](threading-objects-and-features.md)
### [Pool de threads managés](the-managed-thread-pool.md)
### [Minuteries](timers.md)
### [EventWaitHandle, AutoResetEvent, CountdownEvent, ManualResetEvent](eventwaithandle-autoresetevent-countdownevent-manualresetevent.md)
#### [EventWaitHandle](eventwaithandle.md)
#### [AutoResetEvent](autoresetevent.md)
#### [ManualResetEvent and ManualResetEventSlim](manualresetevent-and-manualreseteventslim.md)
#### [CountdownEvent](countdownevent.md)
### [Mutex](mutexes.md)
### [Opérations verrouillées](interlocked-operations.md)
### [Verrous de lecteur-writer](reader-writer-locks.md)
### [Semaphore et SemaphoreSlim](semaphore-and-semaphoreslim.md)
### [Vue d’ensemble des primitives de synchronisation](overview-of-synchronization-primitives.md)
### [Barrier](barrier.md)
#### [Guide pratique pour synchroniser des opérations simultanées avec un objet Barrier](how-to-synchronize-concurrent-operations-with-a-barrier.md)
### [SpinLock](spinlock.md)
#### [Guide pratique pour utiliser le verrouillage SpinLock pour une synchronisation de bas niveau](how-to-use-spinlock-for-low-level-synchronization.md)
#### [Guide pratique pour activer le Mode de suivi des threads dans le verrouillage SpinLock](how-to-enable-thread-tracking-mode-in-spinlock.md)
### [SpinWait](spinwait.md)
#### [Guide pratique pour utiliser SpinWait pour implémenter une opération d’attente en deux phases](how-to-use-spinwait-to-implement-a-two-phase-wait-operation.md)
