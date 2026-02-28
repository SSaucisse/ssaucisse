import React, { useEffect, useState } from "react";
import { Card, CardContent } from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { Compass } from "lucide-react";
import { motion } from "framer-motion";

export default function CompassApp() {
  const [heading, setHeading] = useState(0);
  const [supported, setSupported] = useState(true);

  useEffect(() => {
    if (typeof window !== "undefined" && window.DeviceOrientationEvent) {
      window.addEventListener("deviceorientation", handleOrientation);
    } else {
      setSupported(false);
    }

    return () => {
      window.removeEventListener("deviceorientation", handleOrientation);
    };
  }, []);

  const handleOrientation = (event) => {
    if (event.alpha !== null) {
      setHeading(Math.round(event.alpha));
    }
  };

  const getDirection = (deg) => {
    if (deg >= 337.5 || deg < 22.5) return "Nord";
    if (deg >= 22.5 && deg < 67.5) return "Nord-Est";
    if (deg >= 67.5 && deg < 112.5) return "Est";
    if (deg >= 112.5 && deg < 157.5) return "Sud-Est";
    if (deg >= 157.5 && deg < 202.5) return "Sud";
    if (deg >= 202.5 && deg < 247.5) return "Sud-Ouest";
    if (deg >= 247.5 && deg < 292.5) return "Ouest";
    if (deg >= 292.5 && deg < 337.5) return "Nord-Ouest";
  };

  if (!supported) {
    return (
      <div className="flex items-center justify-center h-screen">
        <p>𝕸𝖆 𝕭𝖔𝖚𝖘𝖘𝖔𝖑𝖊</p>
      </div>
    );
  }

  return (
    <div className="flex items-center justify-center min-h-screen bg-gradient-to-br from-blue sky-900 to-blue sky-600 text-white p-4">
      <Card className="rounded-2xl shadow-2xl p-6 w-full max-w-sm bg-slate-800">
        <CardContent className="flex flex-col items-center gap-6">
          <h1 className="text-2xl font-bold flex items-center gap-2">
            <Compass /> Ma Boussole
          </h1>

          <motion.div
            animate={{ rotate: heading }}
            transition={{ type: "spring", stiffness: 50 }}
            className="w-48 h-48 rounded-full border-4 border-white flex items-center justify-center text-xl font-bold bg-slate-900"
          >
            ↑
          </motion.div>

          <div className="text-center">
            <p className="text-3xl font-semibold">{heading}°</p>
            <p className="text-lg text-gray-300">{getDirection(heading)}</p>
          </div>

          <Button onClick={() => setHeading(0)} className="rounded-2xl">
            Réinitialiser
          </Button>
        </CardContent>
      </Card>
    </div>
  );
}
