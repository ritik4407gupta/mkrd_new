import React, { useState } from 'react';
import { motion, useScroll, useTransform } from 'motion/react';
import { useRef } from 'react';
import {
  Cpu,
  Layers,
  Sparkles,
  Play,
  RotateCcw,
  Zap,
  CheckCircle,
  Activity,
  Sliders,
  Settings
} from 'lucide-react';
import { ThreeDPrinterCanvas } from '../ThreeDPrinterCanvas';
import { ThreeDRobotActuator } from '../ThreeDRobotActuator';

interface SimulationsViewProps {
  onOpenQuoteModal: () => void;
}

export const SimulationsView: React.FC<SimulationsViewProps> = ({ onOpenQuoteModal }) => {
  const [activeSimulationTab, setActiveSimulationTab] = useState<'printer' | 'robot'>('printer');

  const containerRef = useRef<HTMLDivElement>(null);
  const { scrollYProgress } = useScroll({
    target: containerRef,
    offset: ["start start", "end end"]
  });

  // Expand from a standard card size to full screen
    // Advanced Flexible Parallax: scale up, hold, then scale down before exit
  const width = useTransform(scrollYProgress, [0, 0.25, 0.75, 1], ["85vw", "96vw", "96vw", "85vw"]);
  const maxWidth = useTransform(scrollYProgress, [0, 0.25, 0.75, 1], ["75rem", "120rem", "120rem", "75rem"]);
  const height = useTransform(scrollYProgress, [0, 0.25, 0.75, 1], ["60vh", "94vh", "94vh", "60vh"]);
  
  // Keep the edges beautifully curved at all times to give it a floating premium window feel
  const borderRadius = useTransform(scrollYProgress, [0, 0.25, 0.75, 1], ["2rem", "3rem", "3rem", "2rem"]);
  
  // Fade out internal borders and text when it's fully expanded to focus on the 3D model
  const borderOpacity = useTransform(scrollYProgress, [0, 0.25, 0.75, 1], [1, 0.1, 0.1, 1]);
  const textOpacity = useTransform(scrollYProgress, [0, 0.2, 0.8, 1], [1, 0, 0, 1]);
  const shadowIntensity = useTransform(scrollYProgress, [0, 0.25, 0.75, 1], 
    ["0 20px 40px -10px rgba(34,211,238,0.1)", "0 30px 60px -15px rgba(34,211,238,0.4)", "0 30px 60px -15px rgba(34,211,238,0.4)", "0 20px 40px -10px rgba(34,211,238,0.1)"]
  );


  return (
    <div className="space-y-16 pb-16 bg-[#020617] min-h-[100vh] text-slate-300 pt-8">
      {/* Header Banner */}
      <section className="relative overflow-hidden bg-gradient-to-b from-slate-900 to-[#020617] pt-12 pb-8 border-b border-slate-700">
        
        {/* Animated Glow Behind Header */}
        <motion.div 
          initial={{ opacity: 0, scale: 0.8 }}
          animate={{ opacity: 1, scale: 1 }}
          transition={{ duration: 1.5, ease: "easeOut" }}
          className="absolute top-[-10%] left-1/2 -translate-x-1/2 w-[800px] h-[400px] bg-cyan-900/30 blur-[120px] rounded-full pointer-events-none"
        />
        <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 space-y-4 relative z-10">
          <div className="inline-flex items-center gap-2 text-xs font-mono font-bold text-cyan-400">
            <Cpu className="w-4 h-4 text-cyan-400" />
            <span>REAL-TIME BROWSER-BASED HARDWARE KINEMATICS</span>
          </div>
          <motion.h1 initial={{ opacity: 0, y: 30 }} animate={{ opacity: 1, y: 0 }} transition={{ duration: 0.7, delay: 0.2 }} className="text-4xl sm:text-5xl font-display font-extrabold text-white tracking-tight">
            3D SIMULATION <span className="text-cyan-400">LABORATORY</span>
          </motion.h1>
          <motion.p initial={{ opacity: 0, y: 20 }} animate={{ opacity: 1, y: 0 }} transition={{ duration: 0.7, delay: 0.4 }} className="text-base text-slate-400 max-w-2xl leading-relaxed">
            Test and inspect our virtual hardware environments: interactive G-code additive slicer layer deposition and 6-axis industrial robotic arm inverse kinematics.
          </motion.p>

          {/* Simulation Mode Toggle */}
          <div className="flex flex-wrap items-center gap-3 pt-4">
            <button
              onClick={() => setActiveSimulationTab('printer')}
              className={`px-5 py-2.5 rounded-full text-xs font-bold transition-all flex items-center gap-2 ${
                activeSimulationTab === 'printer'
                  ? 'bg-cyan-600 text-white shadow-md shadow-cyan-600/25'
                  : 'bg-slate-900 border border-slate-700 text-slate-400 hover:text-white hover:bg-slate-800'
              }`}
            >
              <Layers className="w-4 h-4" />
              <span>3D Additive Layer & Slicer Simulation</span>
            </button>

            <button
              onClick={() => setActiveSimulationTab('robot')}
              className={`px-5 py-2.5 rounded-full text-xs font-bold transition-all flex items-center gap-2 ${
                activeSimulationTab === 'robot'
                  ? 'bg-cyan-600 text-white shadow-md shadow-cyan-600/25'
                  : 'bg-slate-900 border border-slate-700 text-slate-400 hover:text-white hover:bg-slate-800'
              }`}
            >
              <Cpu className="w-4 h-4" />
              <span>6-Axis Industrial Robotic Arm Kinematics</span>
            </button>
          </div>
        </div>
      </section>

      {/* Active Simulation Stage - Scroll Parallax Container */}
      <section className="w-full relative">
        <div ref={containerRef} className="h-[350vh] relative">
          <div className="sticky top-0 w-full h-screen flex flex-col items-center justify-center overflow-hidden">
            
            <motion.div 
              className="relative bg-[#020617] border-slate-700 overflow-hidden flex flex-col mx-auto"
              style={{ width, maxWidth, height, borderRadius, boxShadow: shadowIntensity }}
            >
              <motion.div style={{ opacity: borderOpacity }} className="absolute inset-0 border border-slate-700 z-10 pointer-events-none rounded-[inherit]" />

              {/* Classy 3D Background Image */}
              <div className="absolute inset-0 z-0">
                <img 
                  src="https://images.unsplash.com/photo-1614729939124-03290b5609ce?q=80&w=2000&auto=format&fit=crop" 
                  alt="3D Background" 
                  className="w-full h-full object-cover opacity-20 pointer-events-none" 
                  crossOrigin="anonymous"
                />
                <div className="absolute inset-0 bg-slate-950/50 backdrop-blur-sm pointer-events-none" />
              </div>

              
              

              {/* The 3D Canvas rendering */}
              <div className="w-full h-full flex-grow relative z-0">
                {activeSimulationTab === 'printer' ? (
                  <ThreeDPrinterCanvas />
                ) : (
                  <ThreeDRobotActuator />
                )}
              </div>
            </motion.div>
          </div>
        </div>
      </section>

      {/* Engineering Assistance Callout */}
      <section className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
        <div className="p-8 rounded-3xl bg-[#0B1121] text-white border border-cyan-800/50 flex flex-col md:flex-row items-center justify-between gap-6 shadow-[0_0_40px_rgba(34,211,238,0.15)] relative overflow-hidden">
          <div className="space-y-2 max-w-xl">
            <h4 className="text-xl font-display font-extrabold">Need Custom Hardware Simulation or Tool DFM?</h4>
            <p className="text-xs text-slate-300 leading-relaxed">
              Our engineering team conducts full Mouldflow rheological analysis, FEA stress simulation, and kinematic path planning for complex tooling and automation lines.
            </p>
          </div>

          <button
            onClick={onOpenQuoteModal}
            className="px-6 py-3.5 rounded-full bg-cyan-400 hover:bg-cyan-300 text-slate-950 font-extrabold text-xs shadow-[0_0_20px_rgba(34,211,238,0.4)] hover:shadow-[0_0_30px_rgba(34,211,238,0.6)] transition-all"
          >
            Consult Engineering Specialist
          </button>
        </div>
      </section>
    </div>
  );
};
