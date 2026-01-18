import React, { useState, useEffect } from 'react';
import { Trophy, Users, Swords, RefreshCw, ChevronRight, Award, History, Scroll } from 'lucide-react';

const Card = ({ children, className = "" }) => (
  <div className={`bg-slate-800 border border-slate-700 rounded-lg shadow-lg ${className}`}>
    {children}
  </div>
);

const Button = ({ children, onClick, variant = "primary", disabled = false, className = "" }) => {
  const baseStyle = "px-4 py-2 rounded font-bold transition-all duration-200 flex items-center justify-center gap-2";
  const variants = {
    primary: "bg-amber-600 hover:bg-amber-500 text-white shadow-[0_0_10px_rgba(217,119,6,0.5)]",
    secondary: "bg-slate-700 hover:bg-slate-600 text-slate-200",
    success: "bg-emerald-600 hover:bg-emerald-500 text-white",
    outline: "border-2 border-slate-600 hover:border-amber-500 text-slate-300 hover:text-amber-500 bg-transparent"
  };

  return (
    <button 
      onClick={onClick} 
      disabled={disabled}
      className={`${baseStyle} ${variants[variant]} ${disabled ? 'opacity-50 cursor-not-allowed' : ''} ${className}`}
    >
      {children}
    </button>
  );
};

export default function YugiohTournament() {
  // Stati principali
  const [setupMode, setSetupMode] = useState(true);
  const [players, setPlayers] = useState([]);
  const [newPlayerName, setNewPlayerName] = useState("");
  const [rounds, setRounds] = useState([]); // Array di array di match
  const [currentRoundIndex, setCurrentRoundIndex] = useState(0);
  const [standings, setStandings] = useState([]);
  const [tournamentFinished, setTournamentFinished] = useState(false);

  // Costanti
  const TOTAL_ROUNDS = 3; // Ottimale per 6-8 giocatori
  const POINTS_WIN = 3;
  const POINTS_DRAW = 1;
  const POINTS_LOSS = 0;

  // Aggiungi giocatore
  const addPlayer = () => {
    if (newPlayerName.trim() === "") return;
    const newPlayer = {
      id: Date.now(),
      name: newPlayerName,
      points: 0,
      wins: 0,
      draws: 0,
      losses: 0,
      matchesPlayed: [], // IDs degli avversari affrontati
    };
    setPlayers([...players, newPlayer]);
    setNewPlayerName("");
  };

  // Rimuovi giocatore (solo in setup)
  const removePlayer = (id) => {
    setPlayers(players.filter(p => p.id !== id));
  };

  // Inizia il torneo
  const startTournament = () => {
    if (players.length < 2) return;
    setSetupMode(false);
    generateRound(1);
  };

  // Algoritmo di pairing Svizzero
  const generateRound = (roundNumber) => {
    let currentPlayers = [...players]; // Copia per manipolazione
    let pairings = [];
    
    // Se è il primo round, mescola casualmente
    if (roundNumber === 1) {
      currentPlayers = currentPlayers.sort(() => Math.random() - 0.5);
    } else {
      // Round successivi: Ordina per punteggio decrescente
      currentPlayers.sort((a, b) => b.points - a.points);
    }

    // Gestione numero dispari (Bye)
    let byePlayer = null;
    if (currentPlayers.length % 2 !== 0) {
      // Prendi l'ultimo giocatore che non ha ancora ricevuto un Bye (semplificato: prende l'ultimo della classifica)
      byePlayer = currentPlayers.pop();
    }

    const playedPairs = new Set(); // Per tracciare chi è stato accoppiato in questo loop

    while (currentPlayers.length > 0) {
      const p1 = currentPlayers.shift();
      let p2Index = -1;

      // Cerca il primo avversario valido (che non ha già affrontato)
      // Nel sistema svizzero stretto, si cerca il punteggio simile.
      // Qui iteriamo per trovare qualcuno che non ha giocato contro p1
      for (let i = 0; i < currentPlayers.length; i++) {
        const potentialOpponent = currentPlayers[i];
        if (!p1.matchesPlayed.includes(potentialOpponent.id)) {
          p2Index = i;
          break;
        }
      }

      // Fallback: se ha giocato contro tutti i rimasti, prendi il primo disponibile (per evitare blocco)
      if (p2Index === -1) {
        p2Index = 0;
      }

      const p2 = currentPlayers.splice(p2Index, 1)[0];
      
      pairings.push({
        id: `R${roundNumber}-M${pairings.length + 1}`,
        p1: p1,
        p2: p2,
        result: null // 'p1', 'p2', 'draw'
      });
    }

    // Se c'è un bye, crea un match fittizio o gestiscilo (qui diamo vittoria automatica)
    if (byePlayer) {
      pairings.push({
        id: `R${roundNumber}-BYE`,
        p1: byePlayer,
        p2: { id: 'bye', name: '--- BYE ---' }, // Dummy
        result: 'p1', // Auto win
        isBye: true
      });
      // Aggiorna subito i punti per il bye
      updatePlayerStats(byePlayer.id, 'win');
    }

    setRounds([...rounds, pairings]);
    setCurrentRoundIndex(roundNumber - 1);
    updateStandings();
  };

  // Gestione click risultato
  const setMatchResult = (roundIdx, matchId, result) => {
    const updatedRounds = [...rounds];
    const match = updatedRounds[roundIdx].find(m => m.id === matchId);
    
    if (match.isBye) return; // Non modificare i bye

    // Se c'era già un risultato, dobbiamo "annullare" i punti prima di applicare i nuovi
    // Per semplicità in questa app, ricalcoliamo tutto lo storico dei giocatori ogni volta che cambia qualcosa
    // Ma per ora, facciamo un approccio semplice: aggiorna lo stato del match e ricalcola classifica globale.
    match.result = result;
    setRounds(updatedRounds);
    recalculateStandings(updatedRounds);
  };

  // Ricalcola classifica basandosi su TUTTI i round
  const recalculateStandings = (allRounds) => {
    // Reset stats
    const tempPlayers = players.map(p => ({
      ...p,
      points: 0,
      wins: 0,
      draws: 0,
      losses: 0,
      matchesPlayed: []
    }));

    allRounds.forEach(round => {
      round.forEach(match => {
        if (!match.result) return;

        const p1 = tempPlayers.find(p => p.id === match.p1.id);
        
        // Gestione BYE
        if (match.isBye) {
           if (p1) {
             p1.points += POINTS_WIN;
             p1.wins += 1;
           }
           return;
        }

        const p2 = tempPlayers.find(p => p.id === match.p2.id);

        if (p1 && p2) {
          // Registra incontro avvenuto
          if (!p1.matchesPlayed.includes(p2.id)) p1.matchesPlayed.push(p2.id);
          if (!p2.matchesPlayed.includes(p1.id)) p2.matchesPlayed.push(p1.id);

          // Assegna punti
          if (match.result === 'p1') {
            p1.points += POINTS_WIN;
            p1.wins += 1;
            p2.losses += 1;
          } else if (match.result === 'p2') {
            p2.points += POINTS_WIN;
            p2.wins += 1;
            p1.losses += 1;
          } else if (match.result === 'draw') {
            p1.points += POINTS_DRAW;
            p1.draws += 1;
            p2.points += POINTS_DRAW;
            p2.draws += 1;
          }
        }
      });
    });

    // Ordina classifica
    tempPlayers.sort((a, b) => {
      if (b.points !== a.points) return b.points - a.points; // Prima i punti
      // Tie breaker semplice: vittorie
      return b.wins - a.wins;
    });

    setPlayers(tempPlayers);
    setStandings(tempPlayers);
  };

  // Helper per aggiornamento rapido (solo per BYE iniziale)
  const updatePlayerStats = (playerId, result) => {
     // Questa logica è gestita meglio dal ricalcolo globale
  };
  
  // Effetto per aggiornare la classifica iniziale
  useEffect(() => {
    setStandings(players);
  }, [setupMode]);

  const handleNextRound = () => {
    const currentRoundMatches = rounds[currentRoundIndex];
    const allMatchesCompleted = currentRoundMatches.every(m => m.result !== null);

    if (!allMatchesCompleted) {
      alert("Completa tutti i risultati delle partite prima di procedere!");
      return;
    }

    if (currentRoundIndex + 1 >= TOTAL_ROUNDS) {
      setTournamentFinished(true);
    } else {
      generateRound(currentRoundIndex + 2);
    }
  };

  // --- RENDER ---

  if (setupMode) {
    return (
      <div className="min-h-screen bg-slate-900 text-slate-100 p-6 font-sans flex flex-col items-center justify-center">
        <div className="max-w-md w-full">
          <div className="text-center mb-8">
            <h1 className="text-4xl font-bold text-amber-500 mb-2 flex items-center justify-center gap-3">
              <Swords size={40} /> Duel Manager
            </h1>
            <p className="text-slate-400">Torneo Yu-Gi-Oh! (Svizzera)</p>
          </div>

          <Card className="p-6">
            <h2 className="text-xl font-bold mb-4 flex items-center gap-2">
              <Users className="text-amber-500" /> Registrazione Duellanti
            </h2>
            
            <div className="flex gap-2 mb-6">
              <input
                type="text"
                value={newPlayerName}
                onChange={(e) => setNewPlayerName(e.target.value)}
                onKeyDown={(e) => e.key === 'Enter' && addPlayer()}
                placeholder="Nome Duellante..."
                className="flex-1 bg-slate-950 border border-slate-700 rounded px-4 py-2 focus:outline-none focus:border-amber-500 text-white"
              />
              <Button onClick={addPlayer} variant="primary">+</Button>
            </div>

            <div className="space-y-2 mb-6 max-h-60 overflow-y-auto">
              {players.length === 0 && <p className="text-center text-slate-600 italic">Nessun duellante iscritto.</p>}
              {players.map((player, idx) => (
                <div key={player.id} className="flex justify-between items-center bg-slate-800 p-3 rounded border border-slate-700">
                  <span className="font-bold text-slate-200">#{idx + 1} {player.name}</span>
                  <button onClick={() => removePlayer(player.id)} className="text-red-400 hover:text-red-300">
                    ✕
                  </button>
                </div>
              ))}
            </div>

            <div className="border-t border-slate-700 pt-4 flex justify-between items-center">
              <span className="text-sm text-slate-400">Totale: {players.length} / 6 consigliati</span>
              <Button 
                onClick={startTournament} 
                disabled={players.length < 2}
                variant={players.length >= 2 ? "success" : "secondary"}
                className="w-full ml-4"
              >
                Inizia Torneo
              </Button>
            </div>
          </Card>
        </div>
      </div>
    );
  }

  return (
    <div className="min-h-screen bg-slate-900 text-slate-100 p-4 md:p-8 font-sans">
      <div className="max-w-5xl mx-auto">
        
        {/* Header */}
        <header className="flex justify-between items-center mb-8 border-b border-slate-700 pb-4">
          <h1 className="text-2xl md:text-3xl font-bold text-amber-500 flex items-center gap-3">
            <Swords /> Duel Manager <span className="text-sm text-slate-500 font-normal ml-2">Round {currentRoundIndex + 1} di {TOTAL_ROUNDS}</span>
          </h1>
          {tournamentFinished && (
            <div className="bg-amber-500/20 text-amber-400 px-4 py-1 rounded-full border border-amber-500/50 text-sm font-bold animate-pulse">
              TORNEO TERMINATO
            </div>
          )}
        </header>

        <div className="grid grid-cols-1 lg:grid-cols-3 gap-8">
          
          {/* Colonna Sinistra: Match Correnti */}
          <div className="lg:col-span-2 space-y-6">
            <div className="flex justify-between items-center">
               <h2 className="text-xl font-bold flex items-center gap-2">
                 <Swords className="text-slate-400" /> Partite in Corso
               </h2>
               {!tournamentFinished && (
                 <Button onClick={handleNextRound} variant="primary">
                   Prossimo Round <ChevronRight size={18} />
                 </Button>
               )}
            </div>

            <div className="space-y-4">
              {rounds[currentRoundIndex]?.map((match) => (
                <Card key={match.id} className="p-4 relative overflow-hidden">
                  {/* Decorazione sfondo */}
                  <div className="absolute top-0 left-0 w-1 h-full bg-amber-600"></div>
                  
                  {match.isBye ? (
                     <div className="flex items-center justify-between text-slate-400 italic">
                        <span>{match.p1.name}</span>
                        <span className="bg-slate-700 px-3 py-1 rounded text-xs text-white">Vittoria Automatica (Bye)</span>
                     </div>
                  ) : (
                    <div className="flex flex-col md:flex-row items-center justify-between gap-4">
                      
                      {/* Player 1 */}
                      <div className={`flex-1 text-center md:text-left ${match.result === 'p1' ? 'text-green-400 font-bold' : match.result === 'p2' ? 'text-red-400 opacity-60' : 'text-white'}`}>
                        <div className="text-lg">{match.p1.name}</div>
                        <div className="text-xs text-slate-500">Punti attuali: {match.p1.points}</div>
                      </div>

                      {/* Controlli Risultato */}
                      <div className="flex items-center gap-2">
                         <button 
                            onClick={() => setMatchResult(currentRoundIndex, match.id, 'p1')}
                            className={`px-3 py-1 rounded border ${match.result === 'p1' ? 'bg-green-600 border-green-500 text-white' : 'border-slate-600 text-slate-400 hover:border-green-500'}`}
                         >
                           WIN
                         </button>
                         <button 
                            onClick={() => setMatchResult(currentRoundIndex, match.id, 'draw')}
                            className={`px-3 py-1 rounded border ${match.result === 'draw' ? 'bg-blue-600 border-blue-500 text-white' : 'border-slate-600 text-slate-400 hover:border-blue-500'}`}
                         >
                           DRAW
                         </button>
                         <button 
                            onClick={() => setMatchResult(currentRoundIndex, match.id, 'p2')}
                            className={`px-3 py-1 rounded border ${match.result === 'p2' ? 'bg-green-600 border-green-500 text-white' : 'border-slate-600 text-slate-400 hover:border-green-500'}`}
                         >
                           WIN
                         </button>
                      </div>

                      {/* Player 2 */}
                      <div className={`flex-1 text-center md:text-right ${match.result === 'p2' ? 'text-green-400 font-bold' : match.result === 'p1' ? 'text-red-400 opacity-60' : 'text-white'}`}>
                        <div className="text-lg">{match.p2.name}</div>
                        <div className="text-xs text-slate-500">Punti attuali: {match.p2.points}</div>
                      </div>

                    </div>
                  )}
                </Card>
              ))}
            </div>
            
            {tournamentFinished && (
                <Card className="p-8 text-center bg-gradient-to-b from-slate-800 to-slate-900 border-amber-500/30">
                    <Trophy size={64} className="mx-auto text-amber-500 mb-4 animate-bounce" />
                    <h2 className="text-3xl font-bold text-white mb-2">Torneo Concluso!</h2>
                    <p className="text-slate-400 mb-6">Congratulazioni al vincitore:</p>
                    <div className="text-4xl font-bold text-amber-400 drop-shadow-[0_0_10px_rgba(251,191,36,0.5)]">
                        {standings[0]?.name}
                    </div>
                </Card>
            )}

          </div>

          {/* Colonna Destra: Classifica */}
          <div className="lg:col-span-1">
            <Card className="p-0 sticky top-4">
               <div className="p-4 border-b border-slate-700 bg-slate-800/50 rounded-t-lg">
                 <h3 className="font-bold flex items-center gap-2 text-amber-500">
                    <Award size={20} /> Classifica
                 </h3>
               </div>
               <div className="overflow-hidden">
                 <table className="w-full text-left text-sm">
                   <thead className="bg-slate-900/50 text-slate-400 uppercase text-xs">
                     <tr>
                       <th className="px-4 py-3">#</th>
                       <th className="px-4 py-3">Nome</th>
                       <th className="px-4 py-3 text-center">W-L-D</th>
                       <th className="px-4 py-3 text-right">Pts</th>
                     </tr>
                   </thead>
                   <tbody className="divide-y divide-slate-700">
                     {standings.map((player, idx) => (
                       <tr key={player.id} className={`hover:bg-slate-700/50 transition-colors ${idx === 0 ? 'bg-amber-500/5' : ''}`}>
                         <td className="px-4 py-3 font-mono text-slate-500">{idx + 1}</td>
                         <td className="px-4 py-3 font-bold text-slate-200">
                            {player.name}
                            {idx === 0 && <Trophy size={14} className="inline ml-2 text-amber-500" />}
                         </td>
                         <td className="px-4 py-3 text-center text-slate-400">
                           {player.wins}-{player.losses}-{player.draws}
                         </td>
                         <td className="px-4 py-3 text-right font-bold text-amber-400">{player.points}</td>
                       </tr>
                     ))}
                   </tbody>
                 </table>
               </div>
            </Card>

            {/* Storico Rounds - Opzionale, per debug visivo */}
            <div className="mt-8">
               <h4 className="text-slate-500 text-xs font-bold uppercase mb-3 flex items-center gap-1">
                  <History size={12} /> Rounds Precedenti
               </h4>
               <div className="flex gap-2">
                 {rounds.map((_, idx) => (
                   <button
                    key={idx}
                    onClick={() => setCurrentRoundIndex(idx)}
                    className={`w-8 h-8 rounded-full text-xs font-bold flex items-center justify-center border ${currentRoundIndex === idx ? 'bg-amber-500 border-amber-500 text-slate-900' : 'bg-transparent border-slate-600 text-slate-400 hover:border-slate-400'}`}
                   >
                     {idx + 1}
                   </button>
                 ))}
               </div>
            </div>

          </div>

        </div>
      </div>
    </div>
  );
}
