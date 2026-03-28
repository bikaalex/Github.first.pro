import { useState, useEffect, useRef } from "react";

const FEATURED_ARTISTS = [
  { name: "Burna Boy", country: "Nigeria", genre: "Afrobeats", emoji: "🇳🇬" },
  { name: "Wizkid", country: "Nigeria", genre: "Afropop", emoji: "🇳🇬" },
  { name: "Davido", country: "Nigeria", genre: "Afrobeats", emoji: "🇳🇬" },
  { name: "Tems", country: "Nigeria", genre: "Afro-Soul", emoji: "🇳🇬" },
  { name: "Tiwa Savage", country: "Nigeria", genre: "Afropop", emoji: "🇳🇬" },
  { name: "Diamond Platnumz", country: "Tanzania", genre: "Bongo Flava", emoji: "🇹🇿" },
  { name: "Sauti Sol", country: "Kenya", genre: "Afro-Pop", emoji: "🇰🇪" },
  { name: "Youssou N'Dour", country: "Senegal", genre: "Mbalax", emoji: "🇸🇳" },
  { name: "Fally Ipupa", country: "DRC", genre: "Ndombolo", emoji: "🇨🇩" },
  { name: "Nasty C", country: "South Africa", genre: "Hip-Hop", emoji: "🇿🇦" },
];

const MUSIC_VISUALS = [
  { gradient: "from-orange-600 via-red-500 to-yellow-400", label: "West Africa" },
  { gradient: "from-emerald-600 via-teal-500 to-cyan-400", label: "East Africa" },
  { gradient: "from-purple-700 via-pink-500 to-rose-400", label: "Central Africa" },
  { gradient: "from-amber-600 via-orange-500 to-red-400", label: "Southern Africa" },
];

const SUGGESTIONS = ["Burna Boy", "Wizkid", "Tems", "Davido", "Diamond Platnumz", "Sauti Sol", "Nasty C", "Tiwa Savage"];

export default function BikSound() {
  const [query, setQuery] = useState("");
  const [searching, setSearching] = useState(false);
  const [result, setResult] = useState(null);
  const [showSuggestions, setShowSuggestions] = useState(false);
  const [activeVisual, setActiveVisual] = useState(0);
  const inputRef = useRef(null);

  useEffect(() => {
    const interval = setInterval(() => {
      setActiveVisual(v => (v + 1) % MUSIC_VISUALS.length);
    }, 3000);
    return () => clearInterval(interval);
  }, []);

  const searchArtist = async (name) => {
    setQuery(name);
    setShowSuggestions(false);
    setSearching(true);
    setResult(null);

    try {
      const response = await fetch("https://api.anthropic.com/v1/messages", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({
          model: "claude-sonnet-4-20250514",
          max_tokens: 1000,
          tools: [{ type: "web_search_20250305", name: "web_search" }],
          system: `You are a music encyclopedia specializing in African artists. When given an artist name, search the web and return ONLY a JSON object (no markdown, no backticks) with these exact fields:
{
  "name": "Full artist name",
  "realName": "Real/birth name",
  "country": "Country of origin",
  "genre": "Primary genre(s)",
  "born": "Date and place of birth",
  "activeYears": "Active since year",
  "biography": "2-3 sentence biography",
  "topSongs": ["song1", "song2", "song3"],
  "albums": ["album1", "album2"],
  "awards": "Key awards/achievements in one sentence",
  "management": "Management company or manager name if known, else 'Not publicly available'",
  "booking": "Booking agency or contact if known, else 'Not publicly available'",
  "instagram": "Instagram handle with @ or 'Not found'",
  "twitter": "Twitter/X handle with @ or 'Not found'",
  "website": "Official website URL or 'Not found'",
  "found": true
}
If the artist is not African or not found, return {"found": false, "name": "the searched name"}.`,
          messages: [{ role: "user", content: `Search for African music artist: ${name}` }],
        }),
      });

      const data = await response.json();
      const text = data.content?.filter(b => b.type === "text").map(b => b.text).join("") || "";
      const clean = text.replace(/```json|```/g, "").trim();
      const parsed = JSON.parse(clean);
      setResult(parsed);
    } catch (err) {
      setResult({ found: false, name, error: true });
    } finally {
      setSearching(false);
    }
  };

  const filteredSuggestions = SUGGESTIONS.filter(s =>
    query.length > 0 && s.toLowerCase().includes(query.toLowerCase())
  );

  return (
    <div style={{ fontFamily: "'Georgia', serif", minHeight: "100vh", background: "#0a0a0a", color: "#f5f0e8" }}>
      {/* Header */}
      <header style={{ padding: "24px 28px 0", display: "flex", justifyContent: "space-between", alignItems: "center" }}>
        <div>
          <div style={{ fontSize: 22, fontWeight: 700, letterSpacing: 3, color: "#f5a623", fontFamily: "Georgia, serif" }}>
            BIK SOUND
          </div>
          <div style={{ fontSize: 10, letterSpacing: 4, color: "#888", textTransform: "uppercase", marginTop: 2 }}>
            African Music Encyclopedia
          </div>
        </div>
        <div style={{ display: "flex", gap: 6 }}>
          {["🇳🇬","🇰🇪","🇿🇦","🇸🇳","🇬🇭"].map((f, i) => (
            <span key={i} style={{ fontSize: 18, opacity: 0.7 }}>{f}</span>
          ))}
        </div>
      </header>

      {!result ? (
        <>
          {/* Hero */}
          <div style={{ padding: "40px 28px 0", textAlign: "center" }}>
            <h1 style={{
              fontSize: "clamp(36px, 8vw, 72px)",
              fontWeight: 900,
              lineHeight: 1.1,
              margin: 0,
              background: "linear-gradient(135deg, #f5a623, #e8315a, #f5a623)",
              backgroundSize: "200%",
              WebkitBackgroundClip: "text",
              WebkitTextFillColor: "transparent",
              animation: "shimmer 3s linear infinite",
            }}>
              Africa's Pulse
            </h1>
            <p style={{ color: "#888", fontSize: 14, marginTop: 10, letterSpacing: 1 }}>
              Discover the stories behind Africa's greatest voices
            </p>
          </div>

          {/* Animated Visual Strips */}
          <div style={{ display: "flex", gap: 8, padding: "28px 28px 0", overflow: "hidden" }}>
            {MUSIC_VISUALS.map((v, i) => (
              <div key={i} style={{
                flex: i === activeVisual ? 2.5 : 1,
                height: 120,
                borderRadius: 14,
                background: `linear-gradient(135deg, var(--c1), var(--c2))`,
                backgroundImage: `linear-gradient(135deg, ${v.gradient.replace("from-", "").replace("via-", "").replace("to-", "")})`,
                transition: "flex 0.6s cubic-bezier(.4,0,.2,1)",
                display: "flex",
                alignItems: "flex-end",
                padding: "10px 14px",
                cursor: "pointer",
                position: "relative",
                overflow: "hidden",
              }} onClick={() => setActiveVisual(i)}>
                <div style={{
                  position: "absolute", inset: 0,
                  background: "repeating-linear-gradient(45deg, rgba(255,255,255,0.03) 0px, rgba(255,255,255,0.03) 1px, transparent 1px, transparent 8px)",
                }} />
                <span style={{
                  fontSize: 11, fontWeight: 700, letterSpacing: 2,
                  color: "rgba(255,255,255,0.9)", textTransform: "uppercase",
                  opacity: i === activeVisual ? 1 : 0,
                  transition: "opacity 0.4s",
                }}>{v.label}</span>
              </div>
            ))}
          </div>

          {/* Search */}
          <div style={{ padding: "32px 28px 0", position: "relative" }}>
            <div style={{
              background: "#161616",
              border: "1px solid #2a2a2a",
              borderRadius: 50,
              display: "flex",
              alignItems: "center",
              padding: "0 20px",
              gap: 12,
            }}>
              <span style={{ fontSize: 18 }}>🔍</span>
              <input
                ref={inputRef}
                value={query}
                onChange={e => { setQuery(e.target.value); setShowSuggestions(true); }}
                onKeyDown={e => e.key === "Enter" && query && searchArtist(query)}
                onFocus={() => setShowSuggestions(true)}
                onBlur={() => setTimeout(() => setShowSuggestions(false), 150)}
                placeholder="Search an African artist..."
                style={{
                  flex: 1, background: "none", border: "none", outline: "none",
                  color: "#f5f0e8", fontSize: 16, padding: "16px 0",
                  fontFamily: "Georgia, serif",
                }}
              />
              {query && (
                <button onClick={() => searchArtist(query)} style={{
                  background: "#f5a623", border: "none", borderRadius: 50,
                  padding: "8px 20px", color: "#0a0a0a", fontWeight: 700,
                  fontSize: 13, cursor: "pointer", letterSpacing: 1,
                }}>GO</button>
              )}
            </div>

            {/* Suggestions */}
            {showSuggestions && filteredSuggestions.length > 0 && (
              <div style={{
                position: "absolute", top: "calc(100% - 8px)", left: 28, right: 28,
                background: "#161616", border: "1px solid #2a2a2a", borderRadius: 16,
                overflow: "hidden", zIndex: 10, boxShadow: "0 20px 40px rgba(0,0,0,0.5)",
              }}>
                {filteredSuggestions.map((s, i) => (
                  <div key={i} onMouseDown={() => searchArtist(s)} style={{
                    padding: "12px 20px", cursor: "pointer", fontSize: 14,
                    borderBottom: i < filteredSuggestions.length - 1 ? "1px solid #1f1f1f" : "none",
                    color: "#ccc",
                    transition: "background 0.15s",
                  }}
                    onMouseEnter={e => e.target.style.background = "#1f1f1f"}
                    onMouseLeave={e => e.target.style.background = "none"}
                  >
                    🎵 {s}
                  </div>
                ))}
              </div>
            )}
          </div>

          {/* Featured Artists */}
          <div style={{ padding: "32px 28px 40px" }}>
            <div style={{ fontSize: 10, letterSpacing: 4, color: "#555", textTransform: "uppercase", marginBottom: 16 }}>
              Featured Artists
            </div>
            <div style={{ display: "flex", gap: 10, flexWrap: "wrap" }}>
              {FEATURED_ARTISTS.map((a, i) => (
                <button key={i} onClick={() => searchArtist(a.name)} style={{
                  background: "#161616",
                  border: "1px solid #222",
                  borderRadius: 50,
                  padding: "8px 16px",
                  color: "#ccc",
                  fontSize: 13,
                  cursor: "pointer",
                  display: "flex",
                  alignItems: "center",
                  gap: 6,
                  transition: "all 0.2s",
                }}
                  onMouseEnter={e => { e.currentTarget.style.borderColor = "#f5a623"; e.currentTarget.style.color = "#f5a623"; }}
                  onMouseLeave={e => { e.currentTarget.style.borderColor = "#222"; e.currentTarget.style.color = "#ccc"; }}
                >
                  <span>{a.emoji}</span> {a.name}
                </button>
              ))}
            </div>
          </div>
        </>
      ) : (
        /* Artist Result Page */
        <div style={{ padding: "24px 28px 60px" }}>
          <button onClick={() => { setResult(null); setQuery(""); }} style={{
            background: "none", border: "1px solid #333", borderRadius: 50,
            color: "#888", padding: "8px 18px", cursor: "pointer", fontSize: 13,
            marginBottom: 28, display: "flex", alignItems: "center", gap: 8,
          }}>← Back</button>

          {!result.found ? (
            <div style={{ textAlign: "center", padding: "60px 0" }}>
              <div style={{ fontSize: 48, marginBottom: 16 }}>🎵</div>
              <h2 style={{ color: "#888", fontWeight: 400 }}>No results found for "{result.name}"</h2>
              <p style={{ color: "#555", fontSize: 14 }}>Try searching for a different African artist</p>
            </div>
          ) : (
            <>
              {/* Artist Hero */}
              <div style={{
                background: "linear-gradient(135deg, #1a1000, #1a0810)",
                border: "1px solid #2a1a00",
                borderRadius: 20,
                padding: "32px 28px",
                marginBottom: 20,
                position: "relative",
                overflow: "hidden",
              }}>
                <div style={{
                  position: "absolute", top: -40, right: -40,
                  width: 200, height: 200, borderRadius: "50%",
                  background: "radial-gradient(circle, rgba(245,166,35,0.15), transparent)",
                }} />
                <div style={{ fontSize: 11, letterSpacing: 4, color: "#f5a623", textTransform: "uppercase", marginBottom: 8 }}>
                  {result.country} · {result.genre}
                </div>
                <h1 style={{ fontSize: "clamp(28px, 6vw, 48px)", margin: "0 0 6px", fontWeight: 900 }}>
                  {result.name}
                </h1>
                {result.realName && result.realName !== result.name && (
                  <div style={{ color: "#888", fontSize: 13, marginBottom: 16 }}>Born: {result.realName}</div>
                )}
                <p style={{ color: "#ccc", fontSize: 14, lineHeight: 1.7, maxWidth: 600, margin: 0 }}>
                  {result.biography}
                </p>
              </div>

              {/* Details Grid */}
              <div style={{ display: "grid", gridTemplateColumns: "1fr 1fr", gap: 12, marginBottom: 12 }}>
                <Card title="🎂 Born">
                  <span style={{ color: "#ccc", fontSize: 14 }}>{result.born || "N/A"}</span>
                </Card>
                <Card title="🎸 Active Since">
                  <span style={{ color: "#ccc", fontSize: 14 }}>{result.activeYears || "N/A"}</span>
                </Card>
              </div>

              <Card title="🏆 Awards & Achievements" style={{ marginBottom: 12 }}>
                <p style={{ color: "#ccc", fontSize: 14, margin: 0, lineHeight: 1.6 }}>{result.awards || "N/A"}</p>
              </Card>

              <div style={{ display: "grid", gridTemplateColumns: "1fr 1fr", gap: 12, marginBottom: 12 }}>
                <Card title="🎵 Top Songs">
                  {(result.topSongs || []).map((s, i) => (
                    <div key={i} style={{ color: "#ccc", fontSize: 13, padding: "4px 0", borderBottom: "1px solid #1f1f1f" }}>
                      {i + 1}. {s}
                    </div>
                  ))}
                </Card>
                <Card title="💿 Albums">
                  {(result.albums || []).map((a, i) => (
                    <div key={i} style={{ color: "#ccc", fontSize: 13, padding: "4px 0", borderBottom: "1px solid #1f1f1f" }}>
                      {a}
                    </div>
                  ))}
                </Card>
              </div>

              {/* Contact & Management */}
              <Card title="📋 Management & Booking">
                <Row label="Management" value={result.management} />
                <Row label="Booking" value={result.booking} />
              </Card>

              <Card title="🌐 Social & Contact" style={{ marginTop: 12 }}>
                <Row label="Instagram" value={result.instagram} link={result.instagram?.startsWith("@") ? `https://instagram.com/${result.instagram.slice(1)}` : null} />
                <Row label="Twitter/X" value={result.twitter} link={result.twitter?.startsWith("@") ? `https://twitter.com/${result.twitter.slice(1)}` : null} />
                <Row label="Website" value={result.website} link={result.website?.startsWith("http") ? result.website : null} />
              </Card>

              {/* Search again */}
              <div style={{ marginTop: 24, background: "#0f0f0f", border: "1px solid #1f1f1f", borderRadius: 50, display: "flex", alignItems: "center", padding: "0 20px", gap: 12 }}>
                <span>🔍</span>
                <input
                  value={query}
                  onChange={e => setQuery(e.target.value)}
                  onKeyDown={e => e.key === "Enter" && query && searchArtist(query)}
                  placeholder="Search another artist..."
                  style={{ flex: 1, background: "none", border: "none", outline: "none", color: "#f5f0e8", fontSize: 15, padding: "16px 0", fontFamily: "Georgia, serif" }}
                />
                {query && (
                  <button onClick={() => searchArtist(query)} style={{ background: "#f5a623", border: "none", borderRadius: 50, padding: "8px 20px", color: "#0a0a0a", fontWeight: 700, fontSize: 13, cursor: "pointer" }}>GO</button>
                )}
              </div>
            </>
          )}
        </div>
      )}

      {/* Loading Overlay */}
      {searching && (
        <div style={{
          position: "fixed", inset: 0, background: "rgba(10,10,10,0.9)",
          display: "flex", flexDirection: "column", alignItems: "center", justifyContent: "center",
          zIndex: 100,
        }}>
          <div style={{ fontSize: 48, animation: "spin 1s linear infinite" }}>🎵</div>
          <p style={{ color: "#f5a623", marginTop: 16, letterSpacing: 2, fontSize: 13, textTransform: "uppercase" }}>
            Searching the web...
          </p>
        </div>
      )}

      <style>{`
        @keyframes shimmer { 0%{background-position:0%} 100%{background-position:200%} }
        @keyframes spin { from{transform:rotate(0deg)} to{transform:rotate(360deg)} }
        * { box-sizing: border-box; }
        ::-webkit-scrollbar { width: 4px; }
        ::-webkit-scrollbar-track { background: #0a0a0a; }
        ::-webkit-scrollbar-thumb { background: #333; border-radius: 4px; }
      `}</style>
    </div>
  );
}

function Card({ title, children, style }) {
  return (
    <div style={{
      background: "#111", border: "1px solid #1f1f1f", borderRadius: 16,
      padding: "18px 20px", ...style,
    }}>
      <div style={{ fontSize: 11, letterSpacing: 3, color: "#555", textTransform: "uppercase", marginBottom: 12 }}>{title}</div>
      {children}
    </div>
  );
}

function Row({ label, value, link }) {
  return (
    <div style={{ display: "flex", justifyContent: "space-between", alignItems: "center", padding: "8px 0", borderBottom: "1px solid #1a1a1a" }}>
      <span style={{ color: "#666", fontSize: 12, letterSpacing: 1 }}>{label}</span>
      {link ? (
        <a href={link} target="_blank" rel="noopener noreferrer" style={{ color: "#f5a623", fontSize: 13, textDecoration: "none" }}>{value}</a>
      ) : (
        <span style={{ color: "#ccc", fontSize: 13 }}>{value || "N/A"}</span>
      )}
    </div>
  );
}