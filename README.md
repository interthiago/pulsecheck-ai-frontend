// 📁 pulsecheck-ai-frontend base structure

// ───────────────────────────────────────────
// Root Files
// ───────────────────────────────────────────
README.md                // Docs generated earlier
package.json             // Project metadata & scripts
.gitignore               // Ignore unnecessary files
postcss.config.js        // Tailwind support
vite.config.js           // Vite build setup

tsconfig.json            // TypeScript support (optional)

// ───────────────────────────────────────────
// App Directories
// ───────────────────────────────────────────
/src
  /components
    GuestProfileTile.tsx         // ✅ Sample component file
    GuestProfileTile.test.tsx    // ✅ Sample test file
  /modules               // Feature-based modules (Guestly, PulsePay, etc)
  /hooks                 // Custom React hooks
  /utils                 // Helper functions and constants
  /assets                // Static files (icons, logos, etc)
  /styles                // Tailwind base + global CSS
  /pages                 // Routed pages (if using React Router)
  /layouts               // Shared layouts (e.g. AdminShell)
  /contexts              // React Context providers
  /services              // API or Supabase client logic
  /tests                 // Unit and integration test files
  /mocks                 // Mock data and test utilities

// ───────────────────────────────────────────
// Sample Component: GuestProfileTile.tsx
// ───────────────────────────────────────────
import React from 'react';

interface GuestProfileTileProps {
  name: string;
  mood: '😊' | '😐' | '😠';
  isVIP?: boolean;
  allergies?: string[];
  hasChild?: boolean;
  profileImage?: string;
}

const GuestProfileTile: React.FC<GuestProfileTileProps> = ({
  name,
  mood,
  isVIP = false,
  allergies = [],
  hasChild = false,
  profileImage
}) => {
  return (
    <div className="p-4 bg-white dark:bg-zinc-900 shadow rounded-xl flex items-center space-x-4">
      <img
        src={profileImage || '/default-avatar.png'}
        alt={name}
        className="w-12 h-12 rounded-full object-cover"
      />
      <div>
        <div className="text-lg font-semibold text-gray-900 dark:text-white">{name} {isVIP && <span className="text-yellow-500">★</span>}</div>
        <div className="text-sm text-gray-500 dark:text-gray-300">Mood: {mood}</div>
        {hasChild && <div className="text-xs text-blue-500">Child Present 👶</div>}
        {allergies.length > 0 && (
          <div className="text-xs text-red-500">Allergies: {allergies.join(', ')}</div>
        )}
      </div>
    </div>
  );
};

export default GuestProfileTile;

// ───────────────────────────────────────────
// Sample Test: GuestProfileTile.test.tsx
// ───────────────────────────────────────────
import { render, screen } from '@testing-library/react';
import GuestProfileTile from './GuestProfileTile';

describe('GuestProfileTile', () => {
  it('renders guest name and mood', () => {
    render(<GuestProfileTile name="Thiago Costa" mood="😊" />);
    expect(screen.getByText(/Thiago Costa/)).toBeInTheDocument();
    expect(screen.getByText(/Mood: 😊/)).toBeInTheDocument();
  });

  it('displays VIP star and child icon', () => {
    render(<GuestProfileTile name="VIP Guest" mood="😐" isVIP hasChild />);
    expect(screen.getByText(/★/)).toBeInTheDocument();
    expect(screen.getByText(/Child Present/)).toBeInTheDocument();
  });

  it('renders allergies if provided', () => {
    render(<GuestProfileTile name="Allergy Guy" mood="😠" allergies={['Shellfish', 'Peanuts']} />);
    expect(screen.getByText(/Allergies: Shellfish, Peanuts/)).toBeInTheDocument();
  });
});
