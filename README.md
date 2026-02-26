import React, { useState, useEffect } from "react";
import { Button } from "@/components/ui/button";
import { motion } from "framer-motion";

const movies = [
  "EL REY LEÓN",
  "TITANIC",
  "HARRY POTTER",
  "JURASSIC PARK",
  "STAR WARS",
  "AVENGERS: ENDGAME",
  "SPIDERMAN",
  "FROZEN",
  "TRANSFORMERS",
  "KUNG FU PANDA",
  "AVATAR",
  "BUSCANDO A NEMO",
  "MEGAMENTE",
  "TOY STORY",
  "ALADDIN",
  "RÁPIDOS Y FURIOSOS",
  "LOS JUEGOS DEL HAMBRE",
  "CREPÚSCULO",
  "EL CONJURO",
  "¿QUÉ PASÓ AYER?",
  "BATMAN",
  "BARBIE",
  "SHREK",
  "FORREST GUMP",
  "WICKED",
  "FIVE NIGHTS AT FREDDY’S",
  "VOLVER AL FUTURO",
  "MATRIX",
  "MISIÓN IMPOSIBLE",
  "EL GRAN SHOWMAN",
  "DEADPOOL",
  "INTERESTELAR",
  "OPPENHEIMER",
  "LOS ILUSIONISTAS",
  "PIRATAS DEL CARIBE"
];

export default function RuletaMimicaMobile() {
  const [selected, setSelected] = useState(null);
  const [phase, setPhase] = useState("idle");
  const [countdown, setCountdown] = useState(3);
  const [timer, setTimer] = useState(20);

  const startGame = () => {
    const randomMovie = movies[Math.floor(Math.random() * movies.length)];
    setSelected(randomMovie);
    setPhase("show");

    setTimeout(() => {
      setPhase("countdown");
    }, 3000);
  };

  useEffect(() => {
    if (phase === "countdown") {
      if (countdown > 0) {
        const interval = setInterval(() => {
          setCountdown((prev) => prev - 1);
        }, 1000);
        return () => clearInterval(interval);
      } else {
        setPhase("play");
      }
    }
  }, [phase, countdown]);

  useEffect(() => {
    if (phase === "play") {
      if (timer > 0) {
        const interval = setInterval(() => {
          setTimer((prev) => prev - 1);
        }, 1000);
        return () => clearInterval(interval);
      } else {
        setPhase("idle");
        setCountdown(3);
        setTimer(20);
      }
    }
  }, [phase, timer]);

  return (
    <div className="flex flex-col items-center justify-center h-screen w-screen bg-black text-white overflow-hidden">
      <h1 className="text-xl mb-6 font-bold tracking-wide">
        RULETA MÍMICA
      </h1>

      {phase === "idle" && (
        <Button
          onClick={startGame}
          className="text-2xl px-10 py-6 rounded-2xl"
        >
          GIRAR
        </Button>
      )}

      {phase === "show" && (
        <motion.div
          initial={{ opacity: 0, scale: 0.7 }}
          animate={{ opacity: 1, scale: 1 }}
          transition={{ duration: 0.5 }}
          className="text-3xl text-center px-6 font-bold"
        >
          {selected}
        </motion.div>
      )}

      {phase === "countdown" && (
        <motion.div
          key={countdown}
          initial={{ scale: 0.5, opacity: 0 }}
          animate={{ scale: 1.2, opacity: 1 }}
          transition={{ duration: 0.5 }}
          className="text-7xl font-bold"
        >
          {countdown}
        </motion.div>
      )}

      {phase === "play" && (
        <motion.div
          key={timer}
          initial={{ scale: 0.8 }}
          animate={{ scale: 1 }}
          className="text-8xl font-extrabold"
        >
          {timer}
        </motion.div>
      )}
    </div>
  );
}
