# WinTracker
Butterfly Zippers
import React, { useState } from "react";
import { jsPDF } from "jspdf";

const WinTracker = () => {
  const [wins, setWins] = useState(Array(30).fill(""));

  const handleInputChange = (index, value) => {
    const updatedWins = [...wins];
    updatedWins[index] = value;
    setWins(updatedWins);
  };

  const exportToPDF = () => {
    const doc = new jsPDF();
    doc.setFont("helvetica", "normal");

    doc.text("Win Tracker - 30 Days", 10, 10);
    wins.forEach((win, index) => {
      doc.text(`Day ${index + 1}: ${win || ""}`, 10, 20 + index * 10);
    });

    doc.save("win-tracker.pdf");
  };

  const resetTracker = () => {
    if (window.confirm("Are you sure you want to reset the tracker for a new month?")) {
      setWins(Array(30).fill(""));
    }
  };

  return (
    <div className="p-6 max-w-xl mx-auto bg-white rounded-2xl shadow-lg border border-gray-200">
      <h1 className="text-2xl font-bold text-center text-gray-800 mb-4">
        Win Tracker Worksheet
      </h1>
      <p className="text-gray-600 text-center mb-6">
        Celebrate your progress by recording your daily wins! No matter how small, every win matters.
      </p>

      <div className="grid grid-cols-1 gap-4">
        {Array.from({ length: 30 }).map((_, index) => (
          <div
            key={index}
            className="p-4 bg-gray-50 rounded-lg border border-gray-300"
          >
            <h2 className="text-lg font-semibold text-gray-700">
              Day {index + 1}
            </h2>
            <textarea
              value={wins[index]}
              onChange={(e) => handleInputChange(index, e.target.value)}
              placeholder="Write your wins here..."
              className="w-full mt-2 p-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-indigo-500"
              rows={3}
            />
          </div>
        ))}
      </div>

      <div className="mt-6 flex justify-between">
        <button
          onClick={exportToPDF}
          className="bg-indigo-600 text-white py-2 px-4 rounded-lg font-medium hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-indigo-500"
        >
          Export to PDF
        </button>
        <button
          onClick={resetTracker}
          className="bg-red-600 text-white py-2 px-4 rounded-lg font-medium hover:bg-red-700 focus:outline-none focus:ring-2 focus:ring-red-500"
        >
          Reset Tracker
        </button>
      </div>
    </div>
  );
};

export default WinTracker;
