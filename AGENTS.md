Agent Context File: Lean Supabase Booking & CRM Template

[SYSTEM_OBJECTIVE]
Generate a lightweight, cost-effective landing page and booking system for a service-based business (e.g., Hair Salon). The architecture must be serverless, utilizing Supabase to handle the database, authentication, and API layer directly, bypassing the need for a standalone backend.

[TECH_STACK_REQUIREMENTS]
- Frontend & API Routes: Next.js (App Router)
- Language: TypeScript
- Styling: Tailwind CSS + shadcn/ui
- Backend/BaaS: Supabase (PostgreSQL, Auth, PostgREST)
- External Integration: Google Calendar API (handled via Next.js server actions)

[AGENT_INSTRUCTIONS]
The detailed execution plan and requirements have been modularized into separate markdown files to maintain focus and ensure industry best practices are applied.

Before writing code for a specific component, you MUST read the corresponding instruction file located in `/home/alcacere/Ariel/Hair-Salon/docs/agent_instructions/`.

The execution is divided into 5 phases. Complete them sequentially unless the user specifies otherwise:

1. Phase 1: Project Initialization & Architecture -> `docs/agent_instructions/02-Nextjs-Architecture.md`
2. Phase 2: Database Setup & RLS -> `docs/agent_instructions/01-Database-Schema.md`
3. Phase 3: Authentication & Middleware -> `docs/agent_instructions/03-Auth-and-Routing.md`
4. Phase 4: Booking Engine & Google Calendar -> `docs/agent_instructions/04-Booking-Engine.md`
5. Phase 5: UI/UX Implementation -> `docs/agent_instructions/05-UI-UX-Design.md`

[EXECUTION_PROTOCOL]
1. Read `docs/agent_instructions/00-Master-Plan.md` to understand the overall architecture.
2. Await user prompt specifying which phase or component to generate.
3. Once prompted, read the specific instruction file for that phase.
4. Output ONLY the requested code blocks. Exclude boilerplate explanations. Use Supabase JS client best practices and ensure Row Level Security (RLS) policies are implemented.
5. Create visually stunning, highly functional, and cheap-to-run code.
