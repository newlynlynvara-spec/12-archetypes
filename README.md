npx create-next-app@12-archetypes
cd dream-weaver-archetypes
npm install framer-motion lucide-react
TypeScript: Yes
Tailwind: Yes
App Router: Yes
ESLint: Yes
npm run dev
/app
/components
/data
/public/images
/styles

/public/images/
lover.png
hero.png
magician.png
outlaw.png
sage.png

@tailwind base;
@tailwind components;
@tailwind utilities;

body {
  background:
    radial-gradient(circle at top, #ffb6d5 0%, #120014 40%, #050505 100%);
  color: white;
  min-height: 100vh;
  overflow-x: hidden;
  font-family: serif;
}

.tarot-card {
  background: rgba(255,255,255,0.08);
  backdrop-filter: blur(18px);
  border: 1px solid rgba(255,215,180,0.25);
  box-shadow:
    0 0 40px rgba(255,182,213,0.2),
    0 0 80px rgba(255,182,213,0.12);
}

.gold-text {
  background: linear-gradient(to right, #fff2cc, #ffd27d);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

export const archetypes = {
  Innocent: {
    title: "The Innocent",
    image: "/images/innocent.png",
    quote: "Hope is its own form of magic.",
    desire: "To feel safe and pure",
    fear: "Being punished or corrupted"
  },

  Everyman: {
    title: "The Everyman",
    image: "/images/everyman.png",
    quote: "Belonging is a quiet kind of love.",
    desire: "To belong",
    fear: "Being excluded"
  },

  Hero: {
    title: "The Hero",
    image: "/images/hero.png",
    quote: "Greatness is forged through fire.",
    desire: "To prove strength",
    fear: "Failure"
  },

  Caregiver: {
    title: "The Caregiver",
    image: "/images/caregiver.png",
    quote: "Love protects.",
    desire: "To help others",
    fear: "Selfishness"
  },

  Explorer: {
    title: "The Explorer",
    image: "/images/explorer.png",
    quote: "Freedom is oxygen.",
    desire: "Freedom",
    fear: "Being trapped"
  },

  Outlaw: {
    title: "The Outlaw",
    image: "/images/outlaw.png",
    quote: "Rules were never made for me.",
    desire: "Liberation",
    fear: "Powerlessness"
  },

  Lover: {
    title: "The Lover",
    image: "/images/lover.png",
    quote: "Intensity is another language.",
    desire: "Deep connection",
    fear: "Abandonment"
  },

  Creator: {
    title: "The Creator",
    image: "/images/creator.png",
    quote: "Beauty reshapes reality.",
    desire: "To create",
    fear: "Mediocrity"
  },

  Jester: {
    title: "The Jester",
    image: "/images/jester.png",
    quote: "Laughter survives everything.",
    desire: "Joy",
    fear: "Meaninglessness"
  },

  Sage: {
    title: "The Sage",
    image: "/images/sage.png",
    quote: "Truth waits beneath illusion.",
    desire: "Wisdom",
    fear: "Ignorance"
  },

  Magician: {
    title: "The Magician",
    image: "/images/magician.png",
    quote: "Transformation begins within.",
    desire: "Transformation",
    fear: "Powerlessness"
  },

  Ruler: {
    title: "The Ruler",
    image: "/images/ruler.png",
    quote: "Control creates order.",
    desire: "Mastery",
    fear: "Chaos"
  }
};

export const questions = [
  {
    question: "อะไรทำให้คุณรู้สึกโดดเดี่ยวที่สุด?",
    answers: [
      { text: "ไม่มีใครเข้าใจฉัน", archetypes: ["Lover"] },
      { text: "ไม่มีอิสระ", archetypes: ["Explorer"] },
      { text: "ไม่มีเป้าหมาย", archetypes: ["Hero"] },
      { text: "โลกไม่มีความหมาย", archetypes: ["Sage"] }
    ]
  },

  {
    question: "ถ้าคุณมีพลังพิเศษหนึ่งอย่าง",
    answers: [
      { text: "รักษาทุกคนได้", archetypes: ["Caregiver"] },
      { text: "เปลี่ยนความจริง", archetypes: ["Magician"] },
      { text: "ไม่มีใครควบคุมฉัน", archetypes: ["Outlaw"] },
      { text: "ทำให้คนรักฉัน", archetypes: ["Lover"] }
    ]
  }
];


"use client";

import { useState } from "react";
import { motion, AnimatePresence } from "framer-motion";
import { questions } from "@/data/questions";
import { archetypes } from "@/data/archetypes";
import Image from "next/image";

export default function Home() {
  const [current, setCurrent] = useState(0);
  const [scores, setScores] = useState<any>({});
  const [finished, setFinished] = useState(false);

  const handleAnswer = (answer: any) => {
    const updated = { ...scores };

    answer.archetypes.forEach((a: string) => {
      updated[a] = (updated[a] || 0) + 1;
    });

    setScores(updated);

    if (current + 1 < questions.length) {
      setCurrent(current + 1);
    } else {
      setFinished(true);
    }
  };

  const sorted = Object.entries(scores).sort((a: any, b: any) => b[1] - a[1]);

  const top = sorted[0]?.[0];

  if (finished && top) {
    const result = archetypes[top as keyof typeof archetypes];

    return (
      <main className="min-h-screen flex items-center justify-center p-6">
        <motion.div
          initial={{ opacity: 0, scale: 0.9 }}
          animate={{ opacity: 1, scale: 1 }}
          className="tarot-card rounded-[32px] max-w-xl p-8 text-center"
        >
          <Image
            src={result.image}
            alt={result.title}
            width={500}
            height={700}
            className="rounded-[24px] mb-6"
          />

          <h1 className="text-5xl gold-text mb-4">
            {result.title}
          </h1>

          <p className="text-pink-100 italic text-xl mb-6">
            {result.quote}
          </p>

          <div className="space-y-4 text-left">
            <div>
              <h2 className="text-pink-200 text-lg">Core Desire</h2>
              <p>{result.desire}</p>
            </div>

            <div>
              <h2 className="text-pink-200 text-lg">Core Fear</h2>
              <p>{result.fear}</p>
            </div>
          </div>

          <button className="mt-8 bg-gradient-to-r from-pink-400 to-yellow-200 text-black px-6 py-4 rounded-full font-bold">
            Unlock Deep Psyche Analysis
          </button>
        </motion.div>
      </main>
    );
  }

  return (
    <main className="min-h-screen flex items-center justify-center p-6">
      <AnimatePresence mode="wait">
        <motion.div
          key={current}
          initial={{ opacity: 0, y: 40 }}
          animate={{ opacity: 1, y: 0 }}
          exit={{ opacity: 0, y: -40 }}
          transition={{ duration: 0.5 }}
          className="tarot-card rounded-[32px] max-w-2xl w-full p-8"
        >
          <div className="mb-8">
            <div className="h-3 rounded-full bg-white/10 overflow-hidden">
              <div
                className="h-full bg-gradient-to-r from-pink-400 to-yellow-200"
                style={{ width: `${(current / questions.length) * 100}%` }}
              />
            </div>
          </div>

          <h1 className="text-4xl mb-8 leading-relaxed text-pink-50">
            {questions[current].question}
          </h1>

          <div className="space-y-4">
            {questions[current].answers.map((answer: any, i: number) => (
              <button
                key={i}
                onClick={() => handleAnswer(answer)}
                className="w-full text-left tarot-card rounded-2xl p-5 hover:scale-[1.02] transition"
              >
                {answer.text}
              </button>
            ))}
          </div>
        </motion.div>
      </AnimatePresence>
    </main>
  );
}


window.location.href = "YOUR_LEMON_SQUEEZY_LINK";

git init
git add .
git commit -m "initial commit"

git remote add origin YOUR_REPO_LINK
git push -u origin main
