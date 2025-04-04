import React, { useState } from 'react';

const WZCQuiz = () => {
  const [currentPage, setCurrentPage] = useState('intro'); // intro, question, results
  const [currentQuestion, setCurrentQuestion] = useState(0);
  const [answers, setAnswers] = useState({
    q1: null,
    q2: null,
    q3: null,
    q4: null,
    q5: null,
    q6: null,
    q7: null,
    q8: [],
    q9: [],
    q10: null
  });
  
  const questions = [
    {
      id: 'q1',
      text: 'Israel should recognize all streams of Judaism when it comes to issues like the Law of Return (Aliyah).',
      type: 'single',
      options: [
        'Strongly Agree',
        'Agree',
        'Neutral/Unsure',
        'Disagree',
        'Strongly Disagree'
      ]
    },
    {
      id: 'q2',
      text: 'I live my life in accordance with Torah values and laws.',
      type: 'single',
      options: [
        'Strongly Agree',
        'Agree',
        'Neutral/Unsure',
        'Disagree',
        'Strongly Disagree'
      ]
    },
    {
      id: 'q3',
      text: 'I am concerned about the future of Israeli democracy.',
      type: 'single',
      options: [
        'Strongly Agree',
        'Agree',
        'Neutral/Unsure',
        'Disagree',
        'Strongly Disagree'
      ]
    }
  ];
  
  const handleSingleSelect = (questionId, option) => {
    setAnswers(prev => ({
      ...prev,
      [questionId]: option
    }));
  };
  
  const goToNextQuestion = () => {
    if (currentQuestion < questions.length - 1) {
      setCurrentQuestion(currentQuestion + 1);
    } else {
      setCurrentPage('results');
    }
  };
  
  const goToPrevQuestion = () => {
    if (currentQuestion > 0) {
      setCurrentQuestion(currentQuestion - 1);
    }
  };
  
  const startQuiz = () => {
    setCurrentPage('question');
  };
  
  const resetQuiz = () => {
    setCurrentPage('intro');
    setCurrentQuestion(0);
    setAnswers({
      q1: null,
      q2: null,
      q3: null,
      q4: null,
      q5: null,
      q6: null,
      q7: null,
      q8: [],
      q9: [],
      q10: null
    });
  };
  
  // Sample results for visual demo only
  const demoResults = [
    { name: 'MERCAZ USA', score: 18, description: 'Voice of Conservative/Masorti Judaism, concerned about democracy, accepting all streams of Judaism, and supporting a safe, just, and lasting peace.' },
    { name: 'ANU: A New Union', score: 16, description: 'Young, pluralistic slate supporting two-state solution, democracy, and all streams of Judaism in Israel.' },
    { name: 'Kol Israel', score: 14, description: 'Apolitical and non-denominational, endorsed by Stand With Us and Young Judea, focused on innovation and combating antisemitism in the US.' }
  ];
  
  return (
    <div className="flex items-center justify-center min-h-screen bg-blue-50">
      <div className="w-full max-w-4xl p-8 mx-auto bg-white rounded-lg shadow-lg">
        {/* Header with standard US and Israeli Flags on either side of title */}
        <div className="flex items-center justify-center mb-8">
          {/* US Flag (placeholder in prototype) */}
          <div className="w-16 h-12 mr-4 bg-red-100 border border-gray-300 shadow-sm flex items-center justify-center text-xs text-gray-500">
            US Flag
          </div>
          
          <h1 className="text-3xl font-bold text-blue-800">World Zionist Congress Slate Quiz</h1>
          
          {/* Israeli Flag (placeholder in prototype) */}
          <div className="w-16 h-12 ml-4 bg-blue-100 border border-gray-300 shadow-sm flex items-center justify-center text-xs text-gray-500">
            Israel Flag
          </div>
        </div>
        
        {/* Intro Page */}
        {currentPage === 'intro' && (
          <div className="text-center">
            <h2 className="mb-6 text-2xl font-semibold text-blue-700">Which WZC Slate Aligns With Your Values?</h2>
            <p className="mb-6 text-lg">This quiz will help you identify which World Zionist Congress slate most closely aligns with your values and perspectives.</p>
            <div className="p-4 mb-8 text-left bg-blue-100 rounded-lg">
              <h3 className="mb-2 text-lg font-semibold">Accessibility Features:</h3>
              <ul className="pl-5 list-disc">
                <li>High contrast text for readability</li>
                <li>Screen reader compatible questions and answers</li>
                <li>Keyboard navigation support (use Tab, Space, and Enter)</li>
                <li>Text size adjustable through browser settings</li>
                <li>No time limits or fast-moving elements</li>
              </ul>
            </div>
            <button 
              className="px-8 py-3 text-lg font-semibold text-white transition-colors bg-blue-600 rounded-md hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2" 
              onClick={startQuiz}
              aria-label="Start the quiz"
            >
              Start Quiz
            </button>
          </div>
        )}
        
        {/* Question Page */}
        {currentPage === 'question' && (
          <div>
            {/* Progress bar */}
            <div className="mb-6">
              <div className="flex justify-between mb-1 text-sm font-medium text-blue-700">
                <span>Question {currentQuestion + 1} of {questions.length}</span>
                <span>{Math.round(((currentQuestion + 1) / questions.length) * 100)}% Complete</span>
              </div>
              <div className="w-full h-2.5 bg-blue-200 rounded-full">
                <div 
                  className="h-2.5 bg-blue-600 rounded-full" 
                  style={{ width: `${((currentQuestion + 1) / questions.length) * 100}%` }}
                ></div>
              </div>
            </div>
            
            <div className="p-6 mb-6 border-l-4 border-blue-500 bg-blue-50">
              <h2 className="mb-4 text-xl font-semibold text-blue-800">{questions[currentQuestion].text}</h2>
              
              <div className="space-y-3">
                {questions[currentQuestion].type === 'single' ? (
                  // Single select options
                  questions[currentQuestion].options.map((option, index) => (
                    <div key={index} className="flex items-center">
                      <input
                        type="radio"
                        id={`${questions[currentQuestion].id}_${index}`}
                        name={questions[currentQuestion].id}
                        className="w-5 h-5 text-blue-600 border-gray-300 focus:ring-blue-500"
                        checked={answers[questions[currentQuestion].id] === option}
                        onChange={() => handleSingleSelect(questions[currentQuestion].id, option)}
                        aria-label={option}
                      />
                      <label 
                        htmlFor={`${questions[currentQuestion].id}_${index}`}
                        className="ml-3 text-lg font-medium text-gray-700"
                      >
                        {option}
                      </label>
                    </div>
                  ))
                ) : (
                  // Multi-select options would go here
                  <div className="text-gray-700">Multi-select options would appear here for questions 8 and 9.</div>
                )}
              </div>
            </div>
            
            <div className="flex justify-between">
              <button
                className="px-6 py-2 text-sm font-medium text-blue-700 bg-white border border-blue-500 rounded-md hover:bg-blue-50 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2"
                onClick={goToPrevQuestion}
                disabled={currentQuestion === 0}
                aria-label="Previous question"
              >
                Previous
              </button>
              <button
                className="px-6 py-2 text-sm font-medium text-white bg-blue-600 border border-blue-600 rounded-md hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2"
                onClick={goToNextQuestion}
                aria-label={currentQuestion < questions.length - 1 ? "Next question" : "See results"}
              >
                {currentQuestion < questions.length - 1 ? 'Next' : 'See Results'}
              </button>
            </div>
          </div>
        )}
        
        {/* Results Page */}
        {currentPage === 'results' && (
          <div>
            <h2 className="mb-6 text-2xl font-semibold text-center text-blue-800">Your Results</h2>
            <p className="mb-6 text-center text-lg text-gray-700">Based on your answers, here are the slates that most closely align with your values:</p>
            
            <div className="mb-8 space-y-6">
              {demoResults.map((result, index) => (
                <div key={index} className="p-6 bg-white border-l-4 border-blue-500 rounded-lg shadow-md">
                  <div className="flex items-start justify-between">
                    <div>
                      <h3 className="text-xl font-bold text-blue-700">{index + 1}. {result.name}</h3>
                      <p className="text-gray-700">{result.description}</p>
                    </div>
                    <div className="flex items-center justify-center w-16 h-16 bg-blue-100 rounded-full">
                      <span className="text-xl font-bold text-blue-700">{result.score}</span>
                    </div>
                  </div>
                </div>
              ))}
            </div>
            
            <div className="p-4 mb-6 bg-yellow-50 border-l-4 border-yellow-400 rounded-lg">
              <div className="flex">
                <div className="flex-shrink-0">
                  <svg className="w-5 h-5 text-yellow-400" viewBox="0 0 20 20" fill="currentColor">
                    <path fillRule="evenodd" d="M18 10a8 8 0 11-16 0 8 8 0 0116 0zm-7-4a1 1 0 11-2 0 1 1 0 012 0zM9 9a1 1 0 000 2v3a1 1 0 001 1h1a1 1 0 100-2h-1V9z" clipRule="evenodd" />
                  </svg>
                </div>
                <div className="ml-3">
                  <p className="text-sm text-yellow-700">
                    Note: These are sample results only. In an actual implementation, results would be calculated based on your specific answers to all questions.
                  </p>
                </div>
              </div>
            </div>
            
            <div className="flex flex-col items-center space-y-4">
              <a 
                href="https://azm.org/elections/"
                target="_blank"
                rel="noopener noreferrer"
                className="px-8 py-3 w-full md:w-auto text-lg font-bold text-white text-center transition-colors bg-green-600 rounded-md hover:bg-green-700 focus:outline-none focus:ring-2 focus:ring-green-500 focus:ring-offset-2"
                aria-label="Vote in the World Zionist Congress elections"
              >
                VOTE NOW in the WZC Elections
              </a>
              
              <button 
                className="px-8 py-3 w-full md:w-auto text-lg font-semibold text-white transition-colors bg-blue-600 rounded-md hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2"
                onClick={() => window.print()}
                aria-label="Print your results"
              >
                Print Results
              </button>
              
              <button 
                className="px-8 py-3 w-full md:w-auto text-lg font-semibold text-blue-700 transition-colors bg-white border border-blue-500 rounded-md hover:bg-blue-50 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2"
                onClick={resetQuiz}
                aria-label="Take the quiz again"
              >
                Take Quiz Again
              </button>
              
              <a 
                href="https://azm.org/elections/"
                target="_blank"
                rel="noopener noreferrer"
                className="text-blue-600 hover:underline focus:outline-none focus:ring-2 focus:ring-blue-500 focus:rounded-sm"
                aria-label="Learn more about the World Zionist Congress elections"
              >
                Learn more about the World Zionist Congress elections
              </a>
            </div>
          </div>
        )}
        
        {/* Footer with accessibility note and voting link */}
        <div className="mt-12 pt-4 border-t border-gray-200 text-center">
          <p className="text-gray-500 text-sm mb-4">This quiz is designed to be accessible to all users. If you encounter any accessibility issues, please contact us.</p>
          
          <div className="flex justify-center space-x-4 mb-6">
            <button 
              className="text-blue-600 hover:underline focus:outline-none focus:ring-2 focus:ring-blue-500 focus:rounded-sm text-sm"
              onClick={() => alert('Font size would increase')}
              aria-label="Increase text size"
            >
              A+ Increase Text Size
            </button>
            <button 
              className="text-blue-600 hover:underline focus:outline-none focus:ring-2 focus:ring-blue-500 focus:rounded-sm text-sm"
              onClick={() => alert('High contrast mode would toggle')}
              aria-label="Toggle high contrast mode"
            >
              High Contrast Mode
            </button>
          </div>
          
          <div className="flex flex-col items-center">
            <p className="text-lg font-semibold text-blue-800 mb-2">Ready to make your voice heard?</p>
            <a 
              href="https://azm.org/elections/"
              target="_blank" 
              rel="noopener noreferrer"
              className="px-6 py-2 bg-blue-100 text-blue-800 rounded-md border border-blue-300 hover:bg-blue-200 transition-colors duration-200 flex items-center justify-center"
            >
              <span className="font-bold">Vote in the World Zionist Congress Elections</span>
              <svg className="w-4 h-4 ml-2" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                <path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M14 5l7 7m0 0l-7 7m7-7H3"></path>
              </svg>
            </a>
            <p className="mt-1 text-sm text-gray-500">https://azm.org/elections/</p>
          </div>
        </div>
      </div>
    </div>
  );
};

export default WZCQuiz;

