# Janmitra - Community Infrastructure Reporting System

A comprehensive platform for citizens to report infrastructure issues and for administrators to manage and track them in real-time.

## Features Implemented

### ✅ Authentication & Role Management
- **Role Selection**: Users can choose between Admin or Citizen roles during signup
- **Role-based Redirects**: 
  - Admins → `/admin` dashboard
  - Citizens → `/reports` page
- **Profile Dropdown**: Shows user name, email, role, and logout functionality
- **Secure Middleware**: Protects admin routes and handles role-based access

### ✅ Community Reports & Voting System
- **All Reports View**: Shows reports from all users (not just personal)
- **Real-time Voting**: Upvote/downvote functionality with instant UI updates
- **Vote Count Display**: Shows vote counts and sorts by highest votes
- **Real-time Sync**: Live updates across all users using Supabase subscriptions

### ✅ Interactive Map View
- **Leaflet Integration**: Interactive map with custom markers
- **Severity-based Markers**: Color-coded pins (green/yellow/orange/red)
- **Popup Details**: Click markers to see report details
- **Real-time Updates**: New reports appear instantly on the map

### ✅ Admin Dashboard
- **Report Management**: Filter and manage all reports
- **Status Updates**: Change report status (Pending/Processing/Done)
- **Department Management**: Create and manage civic departments
- **Authority Assignment**: Assign reports to specific authorities
- **Analytics**: View platform statistics and health metrics

### ✅ Real-time Notifications
- **Toast Notifications**: Instant notifications for status updates
- **Notification Center**: Bell icon with unread count
- **Real-time Sync**: Notifications appear instantly across all users
- **Status Change Alerts**: Users get notified when their reports are updated

### ✅ Database Schema & Security
- **Row Level Security (RLS)**: Proper access controls
- **Role-based Policies**: Users can only access their own data
- **Admin-only Tables**: Protected admin functionality
- **Audit Logging**: Track all admin actions

### ✅ UI/UX Enhancements
- **Dark/Light Mode**: Toggle between themes
- **Mobile Responsive**: Works perfectly on all devices
- **Smooth Animations**: Polished interactions and transitions
- **Modern Design**: Clean, professional interface

## Database Setup

### 1. Run SQL Scripts in Order

Execute these scripts in your Supabase SQL editor:

```sql
-- 1. Create users table
\i scripts/001_create_users_table.sql

-- 2. Create reports table  
\i scripts/002_create_reports_table.sql

-- 3. Create comments table
\i scripts/003_create_comments_table.sql

-- 4. Create votes table
\i scripts/004_create_votes_table.sql

-- 5. Create storage buckets
\i scripts/005_create_storage_bucket.sql
\i scripts/006_create_avatars_bucket.sql

-- 6. Create admin system
\i scripts/007_create_admin_system.sql

-- 7. Create departments and assignments
\i scripts/008_create_departments_and_assignments.sql

-- 8. Add role system and notifications
\i scripts/009_add_role_system.sql

-- 9. Create first admin (optional)
\i scripts/010_create_first_admin.sql
```

### 2. Environment Variables

Create a `.env.local` file:

```env
NEXT_PUBLIC_SUPABASE_URL=your_supabase_url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your_supabase_anon_key
NEXT_PUBLIC_DEV_SUPABASE_REDIRECT_URL=http://localhost:3000/dashboard
```

### 3. Create First Admin User

1. Sign up with an admin role through the UI
2. Run this SQL query in Supabase (replace `USER_ID` with actual user ID):

```sql
-- Get the user ID from auth.users table
SELECT id, email FROM auth.users WHERE email = 'your_admin_email@example.com';

-- Update their profile role
UPDATE public.profiles 
SET role = 'admin' 
WHERE id = 'USER_ID_FROM_ABOVE';

-- Add to admin_roles table
INSERT INTO public.admin_roles (user_id, role, granted_by, is_active)
VALUES ('USER_ID_FROM_ABOVE', 'admin', 'USER_ID_FROM_ABOVE', true);
```

## Development Setup

### 1. Install Dependencies

```bash
npm install
# or
pnpm install
```

### 2. Run Development Server

```bash
npm run dev
# or
pnpm dev
```

### 3. Open in Browser

Visit [http://localhost:3000](http://localhost:3000)

## Key Components

### Authentication Flow
- **Signup**: Role selection (Admin/Citizen) → Supabase metadata
- **Login**: Role-based redirect to appropriate dashboard
- **Middleware**: Protects routes and handles redirects

### Real-time Features
- **Supabase Subscriptions**: Live updates for reports, votes, notifications
- **Optimistic Updates**: Instant UI feedback for better UX
- **Toast Notifications**: Real-time status change alerts

### Admin Features
- **Report Moderation**: Filter, search, and manage all reports
- **Status Management**: Update report status with notifications
- **Department Management**: Create and manage civic departments
- **Authority Assignment**: Assign reports to specific authorities

### Voting System
- **One-click Voting**: Toggle vote with visual feedback
- **Real-time Counts**: Live vote count updates
- **User Vote Tracking**: Shows which reports user has voted on

## API Endpoints

The system uses Supabase for all data operations:

- **Reports**: `reports` table with RLS policies
- **Votes**: `votes` table with automatic count updates
- **Notifications**: `notifications` table with real-time subscriptions
- **Profiles**: `profiles` table with role information
- **Admin Roles**: `admin_roles` table for admin management

## Security Features

- **Row Level Security**: All tables have proper RLS policies
- **Role-based Access**: Admin-only functionality is protected
- **Input Validation**: All forms have proper validation
- **Secure Authentication**: Supabase handles auth securely

## Mobile Responsiveness

- **Responsive Design**: Works on all screen sizes
- **Touch-friendly**: Optimized for mobile interactions
- **Mobile Navigation**: Collapsible menu for mobile devices
- **Mobile Notifications**: Bell icon works on mobile

## Performance Optimizations

- **Real-time Subscriptions**: Efficient Supabase real-time
- **Optimistic Updates**: Instant UI feedback
- **Lazy Loading**: Components load as needed
- **Efficient Queries**: Optimized database queries

## Deployment

### Vercel (Recommended)

1. Connect your GitHub repository to Vercel
2. Add environment variables in Vercel dashboard
3. Deploy automatically on push

### Other Platforms

The app is a standard Next.js application and can be deployed to any platform that supports Next.js.

## Support

For issues or questions:
1. Check the database setup scripts
2. Verify environment variables
3. Check Supabase RLS policies
4. Review the console for errors

## Future Enhancements

- **Email Notifications**: Send emails for status updates
- **SMS Alerts**: Critical issue SMS notifications
- **Advanced Analytics**: More detailed reporting
- **Mobile App**: React Native mobile application
- **API Integration**: Connect with civic department systems
