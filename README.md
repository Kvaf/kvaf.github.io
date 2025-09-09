```jsx
import React, { useState, useEffect } from 'react';

const App = () => {
  const [searchTerm, setSearchTerm] = useState('');
  const [searchResults, setSearchResults] = useState([]);
  const [isSearching, setIsSearching] = useState(false);
  const [activeSection, setActiveSection] = useState('home');
  const [cartItems, setCartItems] = useState([]);
  const [showCart, setShowCart] = useState(false);
  const [mobileMenuOpen, setMobileMenuOpen] = useState(false);

  // Mock domain search function
  const searchDomains = async (term) => {
    setIsSearching(true);
    // Simulate API call
    await new Promise(resolve => setTimeout(resolve, 1500));
    const tlds = ['.com', '.io', '.ai', '.app', '.tech', '.info', '.xyz', '.se', '.nu'];
    const results = tlds.map(tld => ({
      name: term + tld,
      available: Math.random() > 0.3,
      price: (Math.random() * 20 + 5).toFixed(2),
      tld: tld
    }));
    setSearchResults(results);
    setIsSearching(false);
  };

  const addToCart = (domain) => {
    setCartItems([...cartItems, { ...domain, privacy: true, years: 1 }]);
  };

  const removeFromCart = (index) => {
    setCartItems(cartItems.filter((_, i) => i !== index));
  };

  const togglePrivacy = (index) => {
    const newCart = [...cartItems];
    newCart[index].privacy = !newCart[index].privacy;
    setCartItems(newCart);
  };

  const updateYears = (index, years) => {
    const newCart = [...cartItems];
    newCart[index].years = years;
    setCartItems(newCart);
  };

  const getTotal = () => {
    return cartItems.reduce((total, item) => {
      const basePrice = parseFloat(item.price);
      const privacyCost = item.privacy ? 2.99 : 0;
      return total + ((basePrice + privacyCost) * item.years);
    }, 0).toFixed(2);
  };

  // Mock data for services
  const services = [
    {
      icon: "🌐",
      title: "Domain Registration",
      description: "All major TLDs (.com, .io, .ai, .app, .se, .nu, .tech, .info) and yes — if it existed, we'd register .taco",
      features: ["Real-time availability", "Bulk search", "One-click registration", "WHOIS privacy toggle"]
    },
    {
      icon: "💡",
      title: "Domain Brainstorming",
      description: "Spicy suggestions guaranteed to find the perfect name for your brand",
      features: ["AI-powered suggestions", "Brand alignment scoring", "Competitor analysis", "Trademark checks"]
    },
    {
      icon: "🛒",
      title: "Brandable Marketplace",
      description: "Buy and sell premium domains in our secure marketplace",
      features: ["Auction system", "Escrow service", "Premium domains", "Expiring domains"]
    },
    {
      icon: "🚀",
      title: "Web Hosting + Guac",
      description: "Fast, reliable hosting with extra guac (optional but recommended)",
      features: ["SSD storage", "Free SSL", "1-click installs", "24/7 support"]
    },
    {
      icon: "🔒",
      title: "SSL Certificates",
      description: "Because even tacos need security",
      features: ["Free Let's Encrypt", "Auto-install & renew", "Premium OV/EV options", "One-click HTTPS"]
    },
    {
      icon: "📊",
      title: "Analytics & Shortlinks",
      description: "Create branded short URLs with click analytics",
      features: ["go.taco.domains/yourlink", "Vanity subdomains", "Click tracking", "UTM parameters"]
    }
  ];

  // Mock data for testimonials
  const testimonials = [
    {
      name: "Maria Garcia",
      company: "Salsa Startup",
      text: "Taco Domains made setting up our website as easy as ordering tacos. The interface is so intuitive, even my abuela could use it!",
      rating: 5
    },
    {
      name: "Jamal Chen",
      company: "Tech Burrito",
      text: "Switched from GoDaddy and never looked back. The 'Guac Guard' security bundle is worth every penny. Plus, who doesn't love taco-themed error messages?",
      rating: 5
    },
    {
      name: "Sophie Dubois",
      company: "Parisian Tacos",
      text: "The domain brainstorming tool suggested 'parisiantacos.com' - perfect! Customer support responded faster than I could say 'extra salsa'.",
      rating: 5
    }
  ];

  // Mock data for pricing
  const pricingTiers = [
    {
      name: "Single Taco",
      price: "$9.99",
      period: "/year",
      description: "Perfect for personal projects",
      features: [
        "1 domain registration",
        "Basic DNS management",
        "Free SSL certificate",
        "Email forwarding",
        "Standard support"
      ],
      popular: false
    },
    {
      name: "Taco Combo",
      price: "$24.99",
      period: "/year",
      description: "Best value for small businesses",
      features: [
        "5 domain registrations",
        "Advanced DNS management",
        "Free SSL certificates",
        "Email forwarding + MX setup",
        "Priority support",
        "Basic analytics"
      ],
      popular: true
    },
    {
      name: "Taco Feast",
      price: "$49.99",
      period: "/year",
      description: "For agencies and enterprises",
      features: [
        "Unlimited domains",
        "Premium DNS with Anycast",
        "SSL certificates (all types)",
        "Advanced email routing",
        "24/7 priority support",
        "Advanced analytics",
        "API access",
        "Team collaboration"
      ],
      popular: false
    }
  ];

  return (
    <div className="min-h-screen bg-black text-white">
      {/* Navigation */}
      <nav className="bg-black border-b border-gray-700 sticky top-0 z-50">
        <div className="px-4 py-3">
          <div className="flex justify-between items-center">
            <div className="flex items-center">
              <div className="text-2xl mr-2">🌮</div>
              <div className="text-xl font-bold text-white">Taco Domains</div>
            </div>
            <div className="flex items-center space-x-3">
              <button 
                onClick={() => setShowCart(!showCart)}
                className="relative bg-white text-black px-3 py-1.5 rounded-md text-sm hover:bg-gray-200 transition-colors"
              >
                Cart 🛒
                {cartItems.length > 0 && (
                  <span className="absolute -top-1 -right-1 bg-red-500 text-white text-xs rounded-full h-4 w-4 flex items-center justify-center text-[10px]">
                    {cartItems.length}
                  </span>
                )}
              </button>
              <button 
                onClick={() => setMobileMenuOpen(!mobileMenuOpen)}
                className="text-white p-2 hover:bg-gray-800 rounded-md transition-colors md:hidden"
              >
                <svg xmlns="http://www.w3.org/2000/svg" className="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                  <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M4 6h16M4 12h16M4 18h16" />
                </svg>
              </button>
            </div>
          </div>
          
          {/* Mobile Menu */}
          {mobileMenuOpen && (
            <div className="md:hidden mt-3 pb-3 border-t border-gray-700">
              <div className="flex flex-col space-y-2">
                <button 
                  onClick={() => {setActiveSection('home'); setMobileMenuOpen(false);}}
                  className={`font-medium py-2 px-3 rounded-md transition-colors ${activeSection === 'home' ? 'bg-gray-800 text-white' : 'text-gray-300 hover:bg-gray-800 hover:text-white'}`}
                >
                  Home
                </button>
                <button 
                  onClick={() => {setActiveSection('services'); setMobileMenuOpen(false);}}
                  className={`font-medium py-2 px-3 rounded-md transition-colors ${activeSection === 'services' ? 'bg-gray-800 text-white' : 'text-gray-300 hover:bg-gray-800 hover:text-white'}`}
                >
                  Services
                </button>
                <button 
                  onClick={() => {setActiveSection('pricing'); setMobileMenuOpen(false);}}
                  className={`font-medium py-2 px-3 rounded-md transition-colors ${activeSection === 'pricing' ? 'bg-gray-800 text-white' : 'text-gray-300 hover:bg-gray-800 hover:text-white'}`}
                >
                  Pricing
                </button>
                <button 
                  onClick={() => {setActiveSection('marketplace'); setMobileMenuOpen(false);}}
                  className={`font-medium py-2 px-3 rounded-md transition-colors ${activeSection === 'marketplace' ? 'bg-gray-800 text-white' : 'text-gray-300 hover:bg-gray-800 hover:text-white'}`}
                >
                  Marketplace
                </button>
                <button 
                  onClick={() => {setActiveSection('support'); setMobileMenuOpen(false);}}
                  className={`font-medium py-2 px-3 rounded-md transition-colors ${activeSection === 'support' ? 'bg-gray-800 text-white' : 'text-gray-300 hover:bg-gray-800 hover:text-white'}`}
                >
                  Support
                </button>
                <button className="bg-transparent border border-white text-white px-3 py-2 rounded-md hover:bg-white hover:text-black transition-colors mt-2">
                  Login
                </button>
              </div>
            </div>
          )}
          
          {/* Desktop Menu (hidden on mobile) */}
          <div className="hidden md:flex space-x-6 mt-3">
            <button 
              onClick={() => setActiveSection('home')}
              className={`font-medium transition-colors ${activeSection === 'home' ? 'text-white' : 'text-gray-300 hover:text-white'}`}
            >
              Home
            </button>
            <button 
              onClick={() => setActiveSection('services')}
              className={`font-medium transition-colors ${activeSection === 'services' ? 'text-white' : 'text-gray-300 hover:text-white'}`}
            >
              Services
            </button>
            <button 
              onClick={() => setActiveSection('pricing')}
              className={`font-medium transition-colors ${activeSection === 'pricing' ? 'text-white' : 'text-gray-300 hover:text-white'}`}
            >
              Pricing
            </button>
            <button 
              onClick={() => setActiveSection('marketplace')}
              className={`font-medium transition-colors ${activeSection === 'marketplace' ? 'text-white' : 'text-gray-300 hover:text-white'}`}
            >
              Marketplace
            </button>
            <button 
              onClick={() => setActiveSection('support')}
              className={`font-medium transition-colors ${activeSection === 'support' ? 'text-white' : 'text-gray-300 hover:text-white'}`}
            >
              Support
            </button>
          </div>
        </div>
      </nav>

      {/* Shopping Cart Sidebar */}
      {showCart && (
        <div className="fixed inset-0 bg-black bg-opacity-70 z-50 flex justify-end">
          <div className="bg-black border-l border-gray-700 w-full max-w-xs h-full overflow-y-auto p-4 text-white">
            <div className="flex justify-between items-center mb-4">
              <h2 className="text-xl font-bold text-white">Your Cart 🛒</h2>
              <button 
                onClick={() => setShowCart(false)}
                className="text-2xl hover:text-gray-400"
              >
                ×
              </button>
            </div>
            {cartItems.length === 0 ? (
              <p className="text-gray-400 text-center py-8">Your cart is empty. Add some domains!</p>
            ) : (
              <>
                {cartItems.map((item, index) => (
                  <div key={index} className="border-b border-gray-700 py-3 mb-3">
                    <div className="flex justify-between">
                      <h3 className="font-semibold text-sm">{item.name}</h3>
                      <button 
                        onClick={() => removeFromCart(index)}
                        className="text-red-400 hover:text-red-300 text-xs"
                      >
                        Remove
                      </button>
                    </div>
                    <div className="flex justify-between items-center mt-2">
                      <div>
                        <span className="text-xs text-gray-400">Years: </span>
                        <select 
                          value={item.years}
                          onChange={(e) => updateYears(index, parseInt(e.target.value))}
                          className="border border-gray-600 rounded px-1.5 py-0.5 text-xs bg-black text-white"
                        >
                          {[1,2,3,4,5].map(year => (
                            <option key={year} value={year} className="bg-black text-white">{year}</option>
                          ))}
                        </select>
                      </div>
                      <div className="flex items-center">
                        <input
                          type="checkbox"
                          checked={item.privacy}
                          onChange={() => togglePrivacy(index)}
                          className="mr-1"
                        />
                        <span className="text-xs text-gray-400">Privacy</span>
                      </div>
                    </div>
                    <div className="mt-1 text-right">
                      <span className="font-semibold text-sm">
                        ${(parseFloat(item.price) + (item.privacy ? 2.99 : 0)) * item.years}
                      </span>
                    </div>
                  </div>
                ))}
                <div className="mt-4 pt-3 border-t border-gray-700">
                  <div className="flex justify-between text-lg font-bold">
                    <span>Total:</span>
                    <span>${getTotal()}</span>
                  </div>
                  <button className="w-full mt-3 bg-white text-black py-2.5 rounded-md text-sm font-medium hover:bg-gray-200 transition-colors">
                    Checkout 🌮
                  </button>
                </div>
              </>
            )}
          </div>
        </div>
      )}

      {/* Hero Section */}
      {activeSection === 'home' && (
        <section className="relative py-6 px-4">
          <div className="text-center">
            <div className="text-4xl mb-3 animate-bounce">🌮</div>
            <h1 className="text-3xl font-bold mb-3">
              WELCOME TO TACO DOMAINS 🌮
            </h1>
            <p className="text-base text-gray-300 mb-6">
              Where your domain game gets spicy, simple, and seriously delicious.
            </p>
            <div className="bg-black border border-gray-700 rounded-xl p-4 mb-6">
              <h2 className="text-xl font-bold mb-3">
                Get Your Domain. No Salsa Required. <span className="text-white">(But Highly Recommended.)</span>
              </h2>
              <div className="flex flex-col gap-3 mt-4">
                <div className="relative">
                  <input
                    type="text"
                    placeholder="Enter your dream domain name..."
                    value={searchTerm}
                    onChange={(e) => setSearchTerm(e.target.value)}
                    className="w-full px-4 py-3 text-base border-2 border-gray-600 rounded-lg focus:outline-none focus:ring-4 focus:ring-gray-700 bg-black text-white"
                  />
                  <div className="absolute right-3 top-1/2 transform -translate-y-1/2 text-gray-500 text-xs">
                    .com .io .ai .app
                  </div>
                </div>
                <button
                  onClick={() => searchDomains(searchTerm)}
                  disabled={!searchTerm || isSearching}
                  className="bg-white text-black font-bold py-3 px-4 rounded-lg text-base transition-colors flex items-center justify-center hover:bg-gray-200 disabled:bg-gray-700 disabled:text-gray-500"
                >
                  {isSearching ? (
                    <div className="animate-spin rounded-full h-5 w-5 border-b-2 border-black mr-2"></div>
                  ) : (
                    <span className="mr-2">🔍</span>
                  )}
                  {isSearching ? 'Searching...' : 'Search Domain'}
                </button>
              </div>
              <button 
                onClick={() => setActiveSection('how-it-works')}
                className="mt-3 text-white hover:text-gray-300 font-medium text-sm transition-colors"
              >
                📖 How It Works
              </button>
            </div>

            {/* Search Results */}
            {searchResults.length > 0 && (
              <div className="bg-black border border-gray-700 rounded-xl p-4 mb-6">
                <h3 className="text-lg font-bold mb-4">Results for "{searchTerm}"</h3>
                <div className="space-y-3">
                  {searchResults.map((domain, index) => (
                    <div key={index} className={`p-3 rounded-lg border-2 ${
                      domain.available 
                        ? 'border-green-700 bg-green-900 bg-opacity-20' 
                        : 'border-red-700 bg-red-900 bg-opacity-20'
                    } flex flex-col`}>
                      <div className="flex justify-between items-start">
                        <div>
                          <div className="font-bold text-base">{domain.name}</div>
                          <div className={`text-xs ${
                            domain.available ? 'text-green-400' : 'text-red-400'
                          }`}>
                            {domain.available ? 'Available!' : 'Taken 😢'}
                          </div>
                        </div>
                        {domain.available && (
                          <div className="text-base font-bold text-white">${domain.price}/yr</div>
                        )}
                      </div>
                      <div className="mt-2">
                        {domain.available ? (
                          <button
                            onClick={() => addToCart(domain)}
                            className="w-full bg-white text-black py-2 rounded-lg text-sm transition-colors hover:bg-gray-200"
                          >
                            Add to Cart 🛒
                          </button>
                        ) : (
                          <button className="w-full bg-gray-700 text-gray-400 py-2 rounded-lg text-sm cursor-not-allowed">
                            Register
                          </button>
                        )}
                      </div>
                    </div>
                  ))}
                </div>
              </div>
            )}

            {/* Taglines */}
            <div className="mt-6 space-y-3">
              <div className="bg-black border border-gray-700 rounded-lg p-3 border-l-4 border-orange-500">
                <p className="text-sm font-bold">"Don't be shellfish — claim your domain today!"</p>
              </div>
              <div className="bg-black border border-gray-700 rounded-lg p-3 border-l-4 border-red-500">
                <p className="text-sm font-bold">"Hot domains. Fresh ideas. Zero crumbs."</p>
              </div>
              <div className="bg-black border border-gray-700 rounded-lg p-3 border-l-4 border-yellow-500">
                <p className="text-sm font-bold">"Your website deserves more than a boring burrito."</p>
              </div>
            </div>
          </div>
        </section>
      )}

      {/* Services Section */}
      {activeSection === 'services' && (
        <section className="py-6 px-4">
          <div className="max-w-md mx-auto">
            <div className="text-center mb-6">
              <h2 className="text-2xl font-bold mb-2">
                Our Delicious Services 🌮
              </h2>
              <p className="text-sm text-gray-300">
                We've packed all the essential domain services with extra guac and a side of salsa.
              </p>
            </div>
            <div className="space-y-4">
              {services.map((service, index) => (
                <div key={index} className="bg-black border border-gray-700 rounded-lg p-4 border-t-4 border-orange-500">
                  <div className="text-2xl mb-2">{service.icon}</div>
                  <h3 className="text-lg font-bold mb-2">{service.title}</h3>
                  <p className="text-gray-300 text-sm mb-4">{service.description}</p>
                  <ul className="space-y-1 mb-4">
                    {service.features.map((feature, i) => (
                      <li key={i} className="flex items-center">
                        <span className="text-green-400 mr-2">✓</span>
                        <span className="text-sm">{feature}</span>
                      </li>
                    ))}
                  </ul>
                  <button className="w-full bg-white text-black py-2 rounded-lg text-sm font-medium transition-colors hover:bg-gray-200">
                    Learn More
                  </button>
                </div>
              ))}
            </div>
          </div>
        </section>
      )}

      {/* How It Works Section */}
      {activeSection === 'how-it-works' && (
        <section className="py-6 px-4">
          <div className="max-w-md mx-auto">
            <div className="text-center mb-6">
              <h2 className="text-2xl font-bold mb-2">
                How It Works 📖
              </h2>
              <p className="text-sm text-gray-300">
                It's as easy as ordering your favorite taco combo!
              </p>
            </div>
            <div className="space-y-6">
              <div className="flex">
                <div className="mr-4">
                  <div className="w-10 h-10 bg-white text-black rounded-full flex items-center justify-center text-sm font-bold">1</div>
                </div>
                <div>
                  <h3 className="text-lg font-bold mb-1">Search Your Perfect Domain</h3>
                  <p className="text-sm text-gray-300">
                    Type in your dream domain name and we'll show you what's available across all major TLDs.
                  </p>
                </div>
              </div>
              <div className="flex">
                <div className="mr-4">
                  <div className="w-10 h-10 bg-white text-black rounded-full flex items-center justify-center text-sm font-bold">2</div>
                </div>
                <div>
                  <h3 className="text-lg font-bold mb-1">Add to Cart & Customize</h3>
                  <p className="text-sm text-gray-300">
                    Choose how many years you want to register for, add WHOIS privacy, and select any additional services.
                  </p>
                </div>
              </div>
              <div className="flex">
                <div className="mr-4">
                  <div className="w-10 h-10 bg-white text-black rounded-full flex items-center justify-center text-sm font-bold">3</div>
                </div>
                <div>
                  <h3 className="text-lg font-bold mb-1">Checkout & Celebrate</h3>
                  <p className="text-sm text-gray-300">
                    Complete your purchase and you're done! Your domain is registered and ready to use.
                  </p>
                </div>
              </div>
            </div>
            <div className="mt-8 text-center">
              <button 
                onClick={() => setActiveSection('home')}
                className="bg-white text-black px-6 py-3 rounded-lg text-base font-bold transition-colors hover:bg-gray-200"
              >
                Start Your Domain Search Now! 🔍
              </button>
            </div>
          </div>
        </section>
      )}

      {/* Pricing Section */}
      {activeSection === 'pricing' && (
        <section className="py-6 px-4">
          <div className="max-w-md mx-auto">
            <div className="text-center mb-6">
              <h2 className="text-2xl font-bold mb-2">
                Pricing That's Not Half-Baked 🌮
              </h2>
              <p className="text-sm text-gray-300">
                Transparent pricing with no hidden fees.
              </p>
            </div>
            <div className="space-y-4">
              {pricingTiers.map((tier, index) => (
                <div key={index} className={`bg-black border rounded-lg p-4 ${
                  tier.popular ? 'border-2 border-orange-500' : 'border border-gray-700'
                }`}>
                  {tier.popular && (
                    <div className="bg-orange-500 text-black text-center py-1 rounded-t-md -mx-4 -mt-4 mb-3 font-bold text-xs">
                      MOST POPULAR
                    </div>
                  )}
                  <h3 className="text-lg font-bold mb-1">{tier.name}</h3>
                  <div className="flex items-baseline mb-3">
                    <span className="text-xl font-bold text-white">{tier.price}</span>
                    <span className="text-gray-400 ml-1 text-xs">{tier.period}</span>
                  </div>
                  <p className="text-gray-300 text-sm mb-4">{tier.description}</p>
                  <ul className="space-y-2 mb-4">
                    {tier.features.map((feature, i) => (
                      <li key={i} className="flex items-start">
                        <span className="text-green-400 mr-2 mt-0.5">✓</span>
                        <span className="text-sm">{feature}</span>
                      </li>
                    ))}
                  </ul>
                  <button className={`w-full py-2.5 rounded-lg font-bold text-sm transition-colors ${
                    tier.popular 
                      ? 'bg-white text-black hover:bg-gray-200' 
                      : 'bg-gray-800 hover:bg-gray-700 text-white border border-gray-600'
                  }`}>
                    Get Started
                  </button>
                </div>
              ))}
            </div>
            <div className="mt-6 bg-gradient-to-r from-orange-500 to-red-500 rounded-lg p-4 text-center text-black">
              <h3 className="text-lg font-bold mb-2">🌮 Taco Tuesday Special! 🌮</h3>
              <p className="text-sm mb-3">Get 20% off any annual plan with code TACOTUESDAY</p>
              <button className="bg-white text-orange-600 hover:bg-gray-100 px-4 py-2 rounded-lg font-bold text-sm transition-colors">
                Claim Your Discount
              </button>
            </div>
          </div>
        </section>
      )}

      {/* Marketplace Section */}
      {activeSection === 'marketplace' && (
        <section className="py-6 px-4">
          <div className="max-w-md mx-auto">
            <div className="text-center mb-6">
              <h2 className="text-2xl font-bold mb-2">
                Brandable Domain Marketplace 🛒
              </h2>
              <p className="text-sm text-gray-300">
                Buy and sell premium domains in our secure marketplace.
              </p>
            </div>
            <div className="space-y-4">
              {[
                { name: "startup.taco", price: "$2,499", type: "Premium", category: "Tech" },
                { name: "designsalsa.com", price: "$1,299", type: "Auction", category: "Design" },
                { name: "ai.nachos", price: "$5,999", type: "Premium", category: "AI" },
                { name: "cloudguac.io", price: "$899", type: "Fixed", category: "Cloud" },
                { name: "tacobot.ai", price: "$3,499", type: "Premium", category: "AI" },
                { name: "web3.taco", price: "$7,999", type: "Premium", category: "Blockchain" }
              ].map((domain, index) => (
                <div key={index} className="bg-black border border-gray-700 rounded-lg p-4">
                  <div className="flex justify-between items-start mb-2">
                    <h3 className="text-base font-bold">{domain.name}</h3>
                    <span className={`px-2 py-0.5 rounded-full text-xs font-bold ${
                      domain.type === "Premium" ? "bg-red-900 text-red-200" :
                      domain.type === "Auction" ? "bg-yellow-900 text-yellow-200" :
                      "bg-green-900 text-green-200"
                    }`}>
                      {domain.type}
                    </span>
                  </div>
                  <div className="mb-3">
                    <span className="text-white font-bold text-base">{domain.price}</span>
                    <div className="text-gray-400 text-xs mt-1">Category: {domain.category}</div>
                  </div>
                  <div className="flex flex-wrap gap-1 mb-3">
                    <span className="px-1.5 py-0.5 bg-gray-800 rounded text-xs text-gray-300">Brandable</span>
                    <span className="px-1.5 py-0.5 bg-gray-800 rounded text-xs text-gray-300">Memorable</span>
                    <span className="px-1.5 py-0.5 bg-gray-800 rounded text-xs text-gray-300">Short</span>
                  </div>
                  <button className="w-full bg-white text-black py-2 rounded-lg text-sm font-medium transition-colors hover:bg-gray-200">
                    {domain.type === "Auction" ? "Place Bid" : "Buy Now"}
                  </button>
                </div>
              ))}
            </div>
            <div className="mt-6 bg-black border border-gray-700 rounded-lg p-4">
              <h3 className="text-lg font-bold mb-3 text-center">Sell Your Domain</h3>
              <p className="text-sm text-gray-300 mb-4 text-center">
                Have a domain you want to sell? List it on our marketplace.
              </p>
              <div className="space-y-3">
                <input 
                  type="text" 
                  placeholder="Your domain name" 
                  className="w-full px-3 py-2 text-sm border border-gray-600 rounded-lg focus:outline-none focus:ring-2 focus:ring-orange-500 bg-black text-white"
                />
                <select className="w-full px-3 py-2 text-sm border border-gray-600 rounded-lg focus:outline-none focus:ring-2 focus:ring-orange-500 bg-black text-white">
                  <option>Listing Type</option>
                  <option>Fixed Price</option>
                  <option>Auction</option>
                  <option>Negotiable</option>
                </select>
                <input 
                  type="text" 
                  placeholder="Asking Price" 
                  className="w-full px-3 py-2 text-sm border border-gray-600 rounded-lg focus:outline-none focus:ring-2 focus:ring-orange-500 bg-black text-white"
                />
                <textarea 
                  placeholder="Domain Description" 
                  rows="3"
                  className="w-full px-3 py-2 text-sm border border-gray-600 rounded-lg focus:outline-none focus:ring-2 focus:ring-orange-500 bg-black text-white"
                ></textarea>
                <button className="w-full bg-white text-black py-3 rounded-lg text-sm font-bold transition-colors hover:bg-gray-200">
                  List Your Domain 🌮
                </button>
              </div>
            </div>
          </div>
        </section>
      )}

      {/* Support Section */}
      {activeSection === 'support' && (
        <section className="py-6 px-4">
          <div className="max-w-md mx-auto">
            <div className="text-center mb-6">
              <h2 className="text-2xl font-bold mb-2">
                Support & Education 📚
              </h2>
              <p className="text-sm text-gray-300">
                Don't worry, we've got your back! From AI chatbots to live chat, we're here to help.
              </p>
            </div>
            <div className="space-y-4 mb-6">
              <div className="bg-black border border-gray-700 rounded-lg p-4">
                <h3 className="text-lg font-bold mb-3">💬 Live Support</h3>
                <div className="space-y-2">
                  <div className="flex items-center">
                    <div className="w-2 h-2 bg-green-500 rounded-full mr-2"></div>
                    <span className="text-sm">AI Chatbot - Available 24/7</span>
                  </div>
                  <div className="flex items-center">
                    <div className="w-2 h-2 bg-green-500 rounded-full mr-2"></div>
                    <span className="text-sm">Live Chat - Mon-Fri 9AM-9PM EST</span>
                  </div>
                  <div className="flex items-center">
                    <div className="w-2 h-2 bg-green-500 rounded-full mr-2"></div>
                    <span className="text-sm">Email Support - response within 24 hours</span>
                  </div>
                </div>
                <button className="w-full mt-4 bg-white text-black py-2 rounded-lg text-sm font-medium transition-colors hover:bg-gray-200">
                  Start Chat Now
                </button>
              </div>
              <div className="bg-black border border-gray-700 rounded-lg p-4">
                <h3 className="text-lg font-bold mb-3">🌮 Taco Tips</h3>
                <p className="text-gray-300 text-sm mb-3">Educational guides and videos</p>
                <div className="space-y-2">
                  <div className="border-l-4 border-orange-500 pl-3 py-1">
                    <h4 className="font-bold text-sm">How to point your domain to Shopify</h4>
                    <p className="text-xs text-gray-400">5 min video tutorial</p>
                  </div>
                  <div className="border-l-4 border-orange-500 pl-3 py-1">
                    <h4 className="font-bold text-sm">Setting up email forwarding</h4>
                    <p className="text-xs text-gray-400">3 min guide</p>
                  </div>
                  <div className="border-l-4 border-orange-500 pl-3 py-1">
                    <h4 className="font-bold text-sm">DNSSEC setup for enhanced security</h4>
                    <p className="text-xs text-gray-400">8 min advanced tutorial</p>
                  </div>
                </div>
                <button className="w-full mt-4 bg-gray-800 hover:bg-gray-700 text-white py-2 rounded-lg text-sm font-medium transition-colors border border-gray-600">
                  View All Tutorials
                </button>
              </div>
            </div>
            <div className="bg-gradient-to-r from-orange-500 to-red-500 rounded-lg p-4 text-black text-center mb-6">
              <h3 className="text-lg font-bold mb-2">🌮 Taco Tuesday Tech Talks 🌮</h3>
              <p className="text-sm mb-3">Join our weekly live sessions every Tuesday at 2PM EST</p>
              <div className="space-y-2">
                <button className="w-full bg-white text-orange-600 hover:bg-gray-100 py-2 rounded-lg font-bold text-sm transition-colors">
                  Register for Next Session
                </button>
                <button className="w-full border-2 border-white text-white hover:bg-white hover:text-orange-600 py-2 rounded-lg font-bold text-sm transition-colors">
                  Watch Past Recordings
                </button>
              </div>
            </div>
            <div className="mt-6">
              <h3 className="text-lg font-bold mb-4 text-center">What Our Customers Say</h3>
              <div className="space-y-4">
                {testimonials.map((testimonial, index) => (
                  <div key={index} className="bg-black border border-gray-700 rounded-lg p-4">
                    <div className="flex mb-2">
                      {[...Array(testimonial.rating)].map((_, i) => (
                        <span key={i} className="text-yellow-400 text-xs">★</span>
                      ))}
                    </div>
                    <p className="text-gray-300 mb-4 text-sm italic">"{testimonial.text}"</p>
                    <div className="flex items-center">
                      <div className="w-10 h-10 bg-orange-900 rounded-full flex items-center justify-center font-bold text-orange-200 text-sm">
                        {testimonial.name.charAt(0)}
                      </div>
                      <div className="ml-3">
                        <div className="font-bold text-sm">{testimonial.name}</div>
                        <div className="text-gray-400 text-xs">{testimonial.company}</div>
                      </div>
                    </div>
                  </div>
                ))}
              </div>
            </div>
          </div>
        </section>
      )}

      {/* Footer */}
      <footer className="bg-black border-t border-gray-700 text-white py-6 px-4">
        <div className="max-w-md mx-auto">
          <div className="flex items-center mb-4">
            <div className="text-2xl mr-2">🌮</div>
            <div className="text-lg font-bold">Taco Domains</div>
          </div>
          <p className="text-gray-400 text-sm mb-4">
            Where your domain game gets spicy, simple, and seriously delicious.
          </p>
          <div className="grid grid-cols-2 gap-4 mb-6 text-sm">
            <div>
              <h4 className="font-bold mb-2">Services</h4>
              <ul className="space-y-1 text-gray-400">
                <li><a href="#" className="hover:text-white transition-colors">Domain Registration</a></li>
                <li><a href="#" className="hover:text-white transition-colors">Domain Transfer</a></li>
                <li><a href="#" className="hover:text-white transition-colors">Web Hosting</a></li>
              </ul>
            </div>
            <div>
              <h4 className="font-bold mb-2">Support</h4>
              <ul className="space-y-1 text-gray-400">
                <li><a href="#" className="hover:text-white transition-colors">Help Center</a></li>
                <li><a href="#" className="hover:text-white transition-colors">Taco Tips</a></li>
                <li><a href="#" className="hover:text-white transition-colors">Contact Us</a></li>
              </ul>
            </div>
          </div>
          <div className="border-t border-gray-700 pt-4">
            <p className="text-gray-400 text-xs mb-2">
              © 2023 Taco Domains. All rights reserved. 🌮
            </p>
            <div className="flex flex-wrap justify-center gap-3 text-xs">
              <a href="#" className="text-gray-400 hover:text-white transition-colors">Terms</a>
              <a href="#" className="text-gray-400 hover:text-white transition-colors">Privacy</a>
              <a href="#" className="text-gray-400 hover:text-white transition-colors">Cookies</a>
              <a href="#" className="text-gray-400 hover:text-white transition-colors">Sitemap</a>
            </div>
          </div>
          <div className="mt-4 text-center text-gray-500 text-xs">
            <p>Don't be shellfish — tell a friend about Taco Domains! 🌮</p>
          </div>
        </div>
      </footer>
    </div>
  );
};

export default App;
```
