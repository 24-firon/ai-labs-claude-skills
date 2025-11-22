# Claude Code Skills – Setup und Roadmap für deine KI-Agentur

## 1. Status jetzt: Was du schon hast

Auf deinen Screenshots sieht man, dass die Anthropic-Skills wie `algorithmic-art`, `artifacts-builder`, `brand-guidelines`, `internal-comms`, `mcp-builder`, `skill-creator`, `slack-gif-creator` und `theme-factory` bereits verfügbar sind.

Außerdem sind in den Integrationen GitHub und Google Drive verbunden, und lokal laufen bereits MCP-Server wie `filesystem`, `memory`, `PDF Tools` und `playwright` im „LOCAL DEV"-Modus.

Dazu kommen deine eigenen High-Level-Skills wie **AI Agent Builder** (Agent-Entwicklung aus Dokumenten) und der **CMO-Skill** (Brand/Content-CMO mit Onboarding), die du als SKILL-Ordner/Skill-Pakete vorliegen hast.

Zusammen mit deinen Agent-Zero-Blueprints und der Agentur-Roadmap ergibt das bereits eine ziemlich komplette Vision für eine KI-Agentur-Infrastruktur.

---

## 2. „Must-have" Skills in Claude Desktop für die nächsten Wochen

Diese Skills solltest du **definitiv aktiv halten** oder als eigene `.skill`-Pakete hochladen, weil sie direkt auf dein Agentur-Ziel einzahlen.

### 1. **skill-creator (Anthropic)**

- **Autor:** Anthropic
- **Quelle/Dateien:** Standard-Anthropic-Skill; dokumentiert in deinen Claude-Code-Skills-Dokumenten
- **Zweck:** Nutzt du, um deine Long-Long-Dokumente („Agent Zero", „Blueprint", Cursor-Ecosystem etc.) in saubere, versionierbare Skills umzuwandeln.
- **Praktischer Use-Case:** Ideal, um aus z.B. „AI-AGENT-BUILDER-README" automatisch eine `ai-agent-builder.skill` zu generieren, statt alles per Hand zu definieren.
- **Empfehlung:** Immer aktiv lassen. Gezielt nutzen, wenn du eine neue Lang-Doku erstellt hast, die künftig als Skill dienen soll.

### 2. **mcp-builder (Anthropic)**

- **Autor:** Anthropic
- **Quelle/Dateien:** Standard-Anthropic-Skill; Zusammenfassungen zu Docker Desktop 4.50, Code Execution und MCP-Report
- **Zweck:** Hilft dir, qualitativ saubere MCP-Server zu designen (z.B. für n8n, Stripe, interne APIs), mit klaren Commands, Schemas und Sicherheitsregeln.
- **Praktischer Use-Case:** Passt perfekt zu deinem Ziel, dass Claude später auf einem headless-Server wie ein „Infra-Brain" für deine Agentur läuft.
- **Empfehlung:** Aktiv nutzen, wenn du neue MCP-Services planst oder bestehende aufräumen/vereinheitlichen willst.

### 3. **AI Agent Builder (deine Skill)**

- **Autor:** Du selbst (Peter / KI-Agentur), basiert auf deinen eigenen Methoden
- **Quelle/Dateien:** AI-AGENT-BUILDER-README.md; mehrere SKILL.md-Dateien (Agenten-Aufbau, Rollen, Workflows)
- **Zweck:** Dieses Skill-Paket enthält bereits einen 6-Schritte-Prozess, Templates, Checklisten und Tools (`validate_prompt.py`, `token_counter.py`), um aus Dokumenten production-ready Agenten zu bauen.
- **Praktischer Use-Case:** Für dich heißt das: Jedes Mal, wenn du in Cursor oder Claude sagst „bau mir einen Agenten für X", kann Claude systematisch System-Prompt, Tests und Doku erzeugen, statt ad-hoc zu improvisieren.
- **Empfehlung:** In Claude Desktop als globaler Skill aktiv halten. Immer verwenden, wenn ein neuer Agent, ein neuer Workflow oder eine neue Rolle für deine Agentur entworfen wird.

### 4. **CMO / Chief-Marketing-Officer Skill (deine Skill)**

- **Autor:** Du, als Marketing-Framework für die Agentur und deine Kunden
- **Quelle/Dateien:** SKILL.md-Varianten mit Marketing-, Brand- und Content-Strukturen; Teile aus Blueprint-für-eine-Agent-Zero-gestützte-KI-Agentur.md (Marketing-/Kommunikationssektionen)
- **Zweck:** Dieses Skill ist ein extrem starkes Onboarding- und Brand-System: Es erzeugt zuerst `brand-identity.md`, `brand-voice.md` und `target-audience.md` und nutzt sie dann für alle Content-Entscheidungen.
- **Praktischer Use-Case:** Damit definierst du EINMAL deine Agentur-Brand und kannst dann alle späteren Website-Texte, Sales-Mails, Loom-Scripts usw. konsistent von Claude erzeugen lassen.
- **Empfehlung:** Skill immer aktiv, wenn Claude irgendeine Außenkommunikation schreibt (Webseite, Sales, E-Mails, Social). Mit AI-AGENT-BUILDER kombinieren, wenn du agentische „CMO-Assistenten" für Kunden bauen willst.

### 5. **Agency-Ops / Agentur-Blueprint-Skill (aus deinen Blueprint-Docs)**

- **Autor:** Du, kombiniert aus mehreren Infra-Dokumenten
- **Quelle/Dateien:** Blueprint-für-eine-Agent-Zero-gestützte-KI-Agentur.md; Docker-Desktop-4.50-Report; Agent-Zero-Plan.md; CLAUDE.md-Peter-Breco-KI-Agentur-Infrastructure-Setup.md; CLAUDE.md-Wichtige-Regeln.md; CLAUDE.md-n8n-workflows-Repository.md; Agent-Zero-Setup-Guide.md; Agent-Zero.md
- **Zweck:** Aus diesen Dokumenten solltest du mit `skill-creator` ein Skill machen, das beschreibt, wie eure Agentur technisch funktionieren soll. Dieses Skill dient dann als „Betriebssystem-Handbuch" für alle anderen Agenten (z.B. GitHub-Setup-Agent, CI-Agent, Kosten-Monitor).
- **Praktischer Use-Case:** Alle Infrastruktur-Entscheidungen (Docker, n8n, GitHub-Workflows, Sicherheitsregeln) sind an einer Stelle dokumentiert und können systematisch von Claude befolgt werden.
- **Empfehlung:** Als eigener Skill in Claude Desktop anlegen („Agency-Infrastructure" o.ä.). Immer aktiv, wenn Claude an Infrastruktur, Server-Setup, n8n-Workflows oder CI/CD arbeitet.

---

## 3. Anthropic-Skills, die optional sind (Nice-to-Have)

- **artifacts-builder, canvas-design, theme-factory, algorithmic-art, slack-gif-creator**
- Autor: Anthropic
- Quelle: Standard-Anthropic-Skills
- Zweck: Kreative Erweiterungen für Visuals, GIFs, UI-Mockups etc.
- Empfehlung: Erst dann aktivieren, wenn dein Kern-Stack (Agenten, Infra, MCP, CI/CD) stabil läuft. Für „Nice-to-Have"-Projekte oder Content-Experimente nutzen.

Die restlichen kreativen Anthropic-Skills sind nice-to-have für Assets, aber nicht kritisch für dein aktuelles Fundament.

---

## 4. Noch zu verpackende Skills (ToDo)

### **Docker/MCP-Operations-Skill**

- Basis: Vollständige Docker-Desktop-4.50- und Code-Execution-Doku (aus deinen Dateien „Vollständige-Zusammenfassung_-Docker-Desktop-4.50-Code-Execution.md" und „Code-Execution-MCP-Report.md")
- Zweck: Alle Best Practices für Docker + MCP + Code Execution gebündelt als ein Skill
- Wann erstellen: Sobald dein lokaler Docker-Stack (n8n + Postgres + MCP Gateway) stabil läuft

### **Claude-Code-Mastery-Skill**

- Basis: Claude-Code-Skills-Agentur-Roadmap.md und Claude-Code-Skills-ULTIMATE-ESSENCE.md
- Zweck: Ein Meta-Skill, der all deine Claude-Code-Betriebserfahrung abbildet und als Lern-/Schulungs-Skill für dich und zukünftige Mitarbeiter dient
- Wann erstellen: Wenn du mehrere Agenten erfolgreich deployed hast und dein Prozess gefestigt ist

---

## 5. Konkrete Nächste Schritte

1. **Heute/Diese Woche:**
   - Claude Desktop öffnen
   - skill-creator Skill aktiv überprüfen
   - AI Agent Builder Skill laden und testen
   - CMO Skill laden und im Kontext mit Brand-Identity-Prompt testen

2. **Nächste Woche:**
   - mcp-builder mit deinen Docker 4.50 Dokumenten testen
   - Agency-Ops-Skill aus deinen Infra-Docs mit skill-creator zusammenpacken
   - Im Claude Desktop testen, ob Claude alle Agency-Regeln befolgt

3. **Phase 2 (wenn PC-Setup stabil ist):**
   - Docker/MCP-Operations-Skill erstellen
   - Claude-Code-Mastery-Skill erstellen
   - Skills auf GitHub hochladen und versionieren

---

## 6. Zusammenfassung: Skill-Matrix

| Skill | Autor | Status | Priorität | Einsatz |
|-------|-------|--------|-----------|---------|
| skill-creator | Anthropic | Verfügbar | MUST | Kontinuierlich, um Docs zu Skills zu konvertieren |
| mcp-builder | Anthropic | Verfügbar | MUST | Beim Design neuer MCP-Server |
| AI Agent Builder | Du | Vorhanden | MUST | Beim Bauen neuer Agenten |
| CMO / Brand-Skill | Du | Vorhanden | MUST | Bei Außenkommunikation / Content |
| Agency-Ops-Skill | Du | Zu erstellen | MUST | Bei Infrastruktur / Server / Workflows |
| Docker/MCP-Ops | Du | Zu erstellen | HOCH | Wenn Docker-Stack läuft |
| Claude-Code-Mastery | Du | Zu erstellen | HOCH | Dokumentation / Schulung |
| Creative Skills | Anthropic | Verfügbar | OPTIONAL | Später, wenn Kern stabil |

---

Diese Datei dient dir als **Live-Roadmap**: Hake Skills ab, wenn du sie aktiviert/getestet hast, und nutze die Nächsten-Schritte als Checkliste für dein Setup in den nächsten Wochen.