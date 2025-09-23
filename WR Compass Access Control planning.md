---
tags:
- WR
- Compass
MOC: Work
---
[[_0000 Home|Home]] | [[_0002 Work MOC|Back to Work MOC]] | [[WR Compass index]]
```typescript
const enum permissions = {
	CRUD_TEAM_OBJECTIVE: crud-team-objective
	CRUD_DEP_OBJ
	
	CAN_VIEW_ALL_TEAMS
	CAN_VIEW_OWN_TEAM
}
```

# Approach
- Level based approach
- Pure permissions approach
# Claude's Idea with my suggestion
based on this discussion with claude, it seems the best approach is a hybrid. We can create this level based approach (Called hierarchical RBAC or inherited RBAC) and use it to make templates. We can also have the individual permissions defined (Called Flat RBAC) and allow the admin when creating roles to choose a template, or make his own role.

The AI also said that this is how ABAC factors in in industry

const policy = {   

effect: "ALLOW",  

 subject: { hierarchyLevel: "TEAM_LEAD" },   

action: ["CREATE", "READ", "UPDATE", "DELETE"],   

resource: { type: "OBJECTIVE" },   

condition: "resource.team.id IN subject.leadsTeams[].id" };

So my recommendation for the hybrid approach:

- Admin can select a predefined template, or adjust an existing one
- Admin can create a role and assign it permissions and an access level (cleareance)
- The role feature should have the following attributes, which will be controllable when creating a role:  
    1. Access Level: a numerical value (level 1 : lowest, level 4: highest) that determines what you can access in the company (level 1: own work only, level 2: team level, level 3: department level, level 4: CEO - full access)   
    2. Available actions: CRUD and other permissions  
    3. Resource (optional we can make this separation but it's not obligatory): What entity the CRUD applies to  
    4. Condition/Context: The ABAC part. Can be a function that returns a boolean value indicating if: User leads the team/department (if team ID is in user's teams table = true) then user can take the available action on the specified team

AI's answer

Excellent question! These are well-known approaches in access control systems. Let me break down the industry standards and recommend the best approach for your OKR system.

## Industry-Standard Approaches

### 1. **Hierarchical RBAC (Level-Based) - Your First Approach**

This is called **Role Hierarchy** or **Hierarchical RBAC** in industry standards (NIST RBAC model):

```typescript

// Example: Department Head inherits Team Lead permissions

DepartmentHead > TeamLead > Employee

// If you have OBJECTIVE_CREATE_DEPARTMENT, you automatically get:

// - OBJECTIVE_CREATE_TEAM  

// - OBJECTIVE_CREATE_INDIVIDUAL

```

### 2. **Flat RBAC (Explicit Permissions) - Your Second Approach**

This is **Flat RBAC** where permissions are explicitly assigned:

```typescript

// Must explicitly assign each permission

role.permissions = [

  Permission.OBJECTIVE_CREATE_DEPARTMENT,

  Permission.OBJECTIVE_CREATE_TEAM,      // Must be explicit

  Permission.OBJECTIVE_CREATE_INDIVIDUAL // Must be explicit

]

```

### 3. **Hybrid Approach (Industry Best Practice)**

Most enterprise systems use a **combination** - this is what I recommend for your OKR system:

## Recommended Implementation: Hybrid ABAC + Hierarchical RBAC

```typescript

// src/shared/permissions/enums/permission.enum.ts

export enum Permission {

  // Explicit scope-based permissions (Flat RBAC)

  OBJECTIVE_CREATE_COMPANY = 'objective:create:company',

  OBJECTIVE_CREATE_DEPARTMENT = 'objective:create:department',

  OBJECTIVE_CREATE_TEAM = 'objective:create:team',

  OBJECTIVE_CREATE_INDIVIDUAL = 'objective:create:individual',

  // Visibility permissions (Context-aware)

  OBJECTIVE_VIEW_PUBLIC = 'objective:view:public',

  OBJECTIVE_VIEW_PRIVATE_INVITED = 'objective:view:private:invited',

  OBJECTIVE_VIEW_HIERARCHY_DOWN = 'objective:view:hierarchy:down',

  OBJECTIVE_VIEW_ALL = 'objective:view:all', // CEO level

  // Review permissions

  REVIEW_LAUNCH_SUBORDINATES = 'review:launch:subordinates',

  REVIEW_VIEW_HIERARCHY_DOWN = 'review:view:hierarchy:down',

  REVIEW_EDIT_SUBORDINATES = 'review:edit:subordinates',

}

// Permission hierarchy mapping (for inherited permissions)

export const PERMISSION_HIERARCHY = {

  [Permission.OBJECTIVE_CREATE_COMPANY]: [

    Permission.OBJECTIVE_CREATE_DEPARTMENT,

    Permission.OBJECTIVE_CREATE_TEAM,

    Permission.OBJECTIVE_CREATE_INDIVIDUAL,

  ],

  [Permission.OBJECTIVE_CREATE_DEPARTMENT]: [

    Permission.OBJECTIVE_CREATE_TEAM,

    Permission.OBJECTIVE_CREATE_INDIVIDUAL,

  ],

  [Permission.OBJECTIVE_CREATE_TEAM]: [

    Permission.OBJECTIVE_CREATE_INDIVIDUAL,

  ],

} as const;

// Organizational hierarchy levels

export enum HierarchyLevel {

  EMPLOYEE = 1,

  TEAM_LEAD = 2,

  DEPARTMENT_HEAD = 3,

  CEO = 4,

}

```

## Core Access Control Service

```typescript

// src/shared/permissions/services/access-control.service.ts

@Injectable()

export class AccessControlService {

  constructor(

    @InjectRepository(User) private userRepo: Repository<User>,

    @InjectRepository(OKR) private okrRepo: Repository<OKR>,

  ) {}

  async hasPermission(

    userId: number,

    permission: Permission,

    context?: AccessContext

  ): Promise<boolean> {

    const user = await this.getUserWithContext(userId);

    // 1. Check explicit permissions first (Flat RBAC)

    const explicitPermissions = this.getUserExplicitPermissions(user);

    if (explicitPermissions.includes(permission)) {

      return this.checkContextualAccess(user, permission, context);

    }

    // 2. Check inherited permissions (Hierarchical RBAC)

    const inheritedPermissions = this.getUserInheritedPermissions(user);

    if (inheritedPermissions.includes(permission)) {

      return this.checkContextualAccess(user, permission, context);

    }

    return false;

  }

  private getUserExplicitPermissions(user: User): Permission[] {

    return user.roles.flatMap(role => role.permissions);

  }

  private getUserInheritedPermissions(user: User): Permission[] {

    const explicitPermissions = this.getUserExplicitPermissions(user);

    const inherited = new Set<Permission>();

    // Add permissions inherited from higher-level permissions

    for (const permission of explicitPermissions) {

      if (PERMISSION_HIERARCHY[permission]) {

        PERMISSION_HIERARCHY[permission].forEach(p => inherited.add(p));

      }

    }

    return Array.from(inherited);

  }

  private async checkContextualAccess(

    user: User,

    permission: Permission,

    context?: AccessContext

  ): Promise<boolean> {

    switch (permission) {

      case Permission.OBJECTIVE_CREATE_DEPARTMENT:

        return this.canCreateDepartmentObjective(user, context);

      case Permission.OBJECTIVE_VIEW_HIERARCHY_DOWN:

        return this.canViewHierarchyDown(user, context);

      case Permission.OBJECTIVE_VIEW_PRIVATE_INVITED:

        return this.canViewPrivateInvited(user, context);

      default:

        return true; // Permission granted, no additional context needed

    }

  }

  // Visibility rules implementation

  async canViewOKR(userId: number, okrId: number): Promise<boolean> {

    const user = await this.getUserWithContext(userId);

    const okr = await this.okrRepo.findOne({

      where: { id: okrId },

      relations: ['creator', 'contributors', 'team', 'department'],

    });

    if (!okr) return false;

    // 1. Public OKRs - visible to everyone

    if (okr.visibility === OKRVisibility.PUBLIC) {

      return true;

    }

    // 2. Private OKRs - only creator and invited contributors

    if (okr.visibility === OKRVisibility.PRIVATE) {

      if (okr.creator.id === userId) return true;

      if (okr.contributors?.some(c => c.id === userId)) return true;

      // Check if user is a parent in hierarchy (can see down)

      return this.isParentInHierarchy(user, okr.creator);

    }

    return false;

  }

  private async canCreateDepartmentObjective(user: User, context?: AccessContext): Promise<boolean> {

    // Must be department head or higher

    if (user.hierarchyLevel >= HierarchyLevel.DEPARTMENT_HEAD) {

      // If context provided, verify it's their department

      if (context?.departmentId) {

        return user.leadsDepartments?.some(d => d.id === context.departmentId) || false;

      }

      return true;

    }

    return false;

  }

  private async canViewHierarchyDown(user: User, context?: AccessContext): Promise<boolean> {

    if (!context?.targetUserId) return false;

    const targetUser = await this.userRepo.findOne({

      where: { id: context.targetUserId },

      relations: ['team', 'department'],

    });

    return this.isParentInHierarchy(user, targetUser);

  }

  private isParentInHierarchy(parent: User, child: User): boolean {

    // CEO sees everyone

    if (parent.hierarchyLevel === HierarchyLevel.CEO) return true;

    // Department head sees department members and team leads

    if (parent.hierarchyLevel === HierarchyLevel.DEPARTMENT_HEAD) {

      // Direct department member

      if (child.department?.departmentHead?.id === parent.id) return true;

      // Team lead under their department

      if (child.team?.department?.departmentHead?.id === parent.id) return true;

      // Team member under their department

      if (child.team?.department?.id === parent.leadsDepartments?.[0]?.id) return true;

    }

    // Team lead sees team members

    if (parent.hierarchyLevel === HierarchyLevel.TEAM_LEAD) {

      return child.team?.teamLead?.id === parent.id;

    }

    return false;

  }

}

interface AccessContext {

  resourceId?: number;

  departmentId?: number;

  teamId?: number;

  targetUserId?: number;

  okrType?: OKRType;

}

```

## Role Templates with Hybrid Permissions

```typescript

// src/auth/seeds/role-templates.seed.ts

export class RoleTemplateSeeder {

  static readonly DEPARTMENT_HEAD_TEMPLATE = {

    name: 'Department Head',

    description: 'Leads a department with multiple teams',

    permissions: [

      // Explicit high-level permissions (inherits lower levels automatically)

      Permission.OBJECTIVE_CREATE_DEPARTMENT,

      Permission.OBJECTIVE_VIEW_HIERARCHY_DOWN,

      Permission.OBJECTIVE_VIEW_PUBLIC,

      // Review permissions

      Permission.REVIEW_LAUNCH_SUBORDINATES,

      Permission.REVIEW_VIEW_HIERARCHY_DOWN,

      Permission.REVIEW_EDIT_SUBORDINATES,

      // Management permissions

      Permission.USER_VIEW,

      Permission.ROLE_VIEW,

    ],

    isTemplate: true, // Available as template for custom roles

  };

  static readonly TEAM_LEAD_TEMPLATE = {

    name: 'Team Lead',

    description: 'Leads a single team',

    permissions: [

      Permission.OBJECTIVE_CREATE_TEAM, // Inherits individual creation

      Permission.OBJECTIVE_VIEW_HIERARCHY_DOWN,

      Permission.OBJECTIVE_VIEW_PUBLIC,

      Permission.REVIEW_LAUNCH_SUBORDINATES,

      Permission.USER_VIEW,

    ],

    isTemplate: true,

  };

  // Predefined roles (cannot be modified)

  static readonly ADMIN_ROLE = {

    name: 'Admin',

    description: 'System administrator',

    permissions: Object.values(Permission), // All permissions

    isCustom: false,

    isTemplate: false,

  };

  static readonly EMPLOYEE_ROLE = {

    name: 'Employee',

    description: 'Basic employee access',

    permissions: [

      Permission.OBJECTIVE_CREATE_INDIVIDUAL,

      Permission.OBJECTIVE_VIEW_PUBLIC,

      Permission.USER_VIEW, // Own profile only

    ],

    isCustom: false,

    isTemplate: false,

  };

}

```

## Frontend Role Creation UI

```typescript

// features/roles/components/CreateRoleForm.tsx

export function CreateRoleForm() {

  const [selectedTemplate, setSelectedTemplate] = useState<string | null>(null);

  const [customPermissions, setCustomPermissions] = useState<Permission[]>([]);

  const availablePermissions = useMemo(() => {

    const grouped = groupPermissionsByScope(Object.values(Permission));

    return grouped;

  }, []);

  const effectivePermissions = useMemo(() => {

    // Show both explicit and inherited permissions

    const explicit = new Set(customPermissions);

    const inherited = new Set<Permission>();

    customPermissions.forEach(permission => {

      if (PERMISSION_HIERARCHY[permission]) {

        PERMISSION_HIERARCHY[permission].forEach(p => inherited.add(p));

      }

    });

    return {

      explicit: Array.from(explicit),

      inherited: Array.from(inherited),

    };

  }, [customPermissions]);

  return (

    <form>

      {/* Template selection */}

      <Select

        label="Start from template (optional)"

        data={roleTemplates}

        onChange={setSelectedTemplate}

      />

      {/* Permission selection with hierarchy visualization */}

      <PermissionSelector

        permissions={availablePermissions}

        selected={customPermissions}

        onChange={setCustomPermissions}

        showInherited={true}

        effectivePermissions={effectivePermissions}

      />

      {/* Preview effective permissions */}

      <PermissionPreview

        explicit={effectivePermissions.explicit}

        inherited={effectivePermissions.inherited}

      />

    </form>

  );

}

```

## Benefits of This Hybrid Approach

1. **Flexibility**: Users can create roles with explicit permissions or use templates

2. **Convenience**: High-level permissions automatically grant related lower-level permissions

3. **Security**: Context-aware checks prevent unauthorized access

4. **Usability**: Clear visualization of what permissions are inherited

5. **Scalability**: Easy to add new permission hierarchies

6. **Industry Standard**: Follows NIST RBAC guidelines

## Other Industry Approaches to Consider

1. **Attribute-Based Access Control (ABAC)**: Uses attributes of user, resource, and environment

2. **Policy-Based Access Control (PBAC)**: Uses rules/policies written in languages like XACML

3. **Relationship-Based Access Control (ReBAC)**: Based on relationships between entities (like your hierarchy)

For your OKR system, the **Hybrid approach** I outlined combines the best of RBAC, ABAC, and ReBAC, giving you the flexibility you need while maintaining security and usability.

----------------------------------------------------------------------------------------------------------------------

Exactly! You've understood it perfectly. Let me clarify how this is handled in the industry:

## Industry Standard: **Hierarchical RBAC + ABAC (Attribute-Based Access Control)**

Your understanding is correct:

1. **Permissions are defined individually** (Flat RBAC foundation)
2. **Hierarchy provides convenient templates** (Hierarchical RBAC for UX)
3. **Context determines scope** (ABAC for runtime decisions)

The **context access control** you're asking about is called **ABAC (Attribute-Based Access Control)** in industry standards. Here's how it works:

## How Context Access Control Works (ABAC)

Instead of just checking "Can this user create objectives?", ABAC checks:

- **Subject attributes**: User's role, hierarchy level, department, team
- **Resource attributes**: Objective's scope (team/department), owner, visibility
- **Environmental attributes**: Time, location, action being performed
- **Relationship attributes**: "Is this user the manager of this team?"

typescript

Copy

`_// Industry pattern: Policy Evaluation Point (PEP) + Policy Decision Point (PDP)_ _// Policy: "Team Leader can CRUD objectives WHERE objective.team = user.leadsTeam"_ const policy = {   effect:` `"ALLOW"``,   subject: { hierarchyLevel:` `"TEAM_LEAD"` `},   action: [``"CREATE"``,` `"READ"``,` `"UPDATE"``,` `"DELETE"``],   resource: { type:` `"OBJECTIVE"` `},   condition:` `"resource.team.id IN subject.leadsTeams[].id"` `};`

## Complete Implementation: Hierarchical RBAC + ABAC

### 1. **Permission Definition (Flat RBAC Foundation)**

typescript

Copy

`_// Each permission is explicit and granular_ export enum Permission {   _// Scope-specific permissions_   OBJECTIVE_MANAGE_COMPANY =` `'objective:manage:company'``,   OBJECTIVE_MANAGE_DEPARTMENT =` `'objective:manage:department'``,  OBJECTIVE_MANAGE_TEAM =` `'objective:manage:team'``,   OBJECTIVE_MANAGE_INDIVIDUAL =` `'objective:manage:individual'``,   _// Visibility permissions_   OBJECTIVE_VIEW_PUBLIC_ALL =` `'objective:view:public:all'``,   OBJECTIVE_VIEW_HIERARCHY_DOWN =` `'objective:view:hierarchy:down'``,   OBJECTIVE_VIEW_PRIVATE_INVITED =` `'objective:view:private:invited'``, } _// Hierarchy for templates (convenience, not enforcement)_ export const PERMISSION_HIERARCHY = {   [Permission.OBJECTIVE_MANAGE_COMPANY]: [     Permission.OBJECTIVE_MANAGE_DEPARTMENT,     Permission.OBJECTIVE_MANAGE_TEAM,     Permission.OBJECTIVE_MANAGE_INDIVIDUAL,   ],   [Permission.OBJECTIVE_MANAGE_DEPARTMENT]: [     Permission.OBJECTIVE_MANAGE_TEAM,     Permission.OBJECTIVE_MANAGE_INDIVIDUAL,   ],   [Permission.OBJECTIVE_MANAGE_TEAM]: [     Permission.OBJECTIVE_MANAGE_INDIVIDUAL,   ], };`

### 2. **ABAC Policy Engine (Industry Standard Pattern)**

typescript

Copy

`_// src/shared/permissions/services/policy-engine.service.ts_ @Injectable() export class PolicyEngineService {   constructor(     @InjectRepository(User) private userRepo: Repository<User>,     @InjectRepository(Objective) private objectiveRepo: Repository<Objective>,   ) {}   async evaluateAccess(request: AccessRequest): Promise<AccessDecision> {     const { userId, action, resourceType, resourceId, context } = request;     _// Load subject attributes (user context)_     const user = await this.loadUserContext(userId);     _// Load resource attributes (if accessing specific resource)_     const resource = resourceId ? await this.loadResourceContext(resourceType, resourceId) : null;     _// Evaluate policies_     for (const policy of this.getApplicablePolicies(action, resourceType)) {       const decision = await this.evaluatePolicy(policy, user, resource, context);       if (decision.effect ===` `'DENY'``) return decision;       if (decision.effect ===` `'ALLOW'``) return decision;     }     return { effect:` `'DENY'``, reason:` `'No applicable policy found'` `};   }   private async evaluatePolicy(     policy: AccessPolicy,    user: UserContext,    resource: ResourceContext | null,     context:` `any`   `): Promise<AccessDecision> {     _// Check subject conditions_     if (!this.matchesSubjectConditions(policy.subject, user)) {       return { effect:` `'NOT_APPLICABLE'` `};     }     _// Check resource conditions (ABAC magic happens here)_     if (resource && !await this.matchesResourceConditions(policy.resource, resource, user)) {       return { effect:` `'DENY'``, reason:` `'Resource conditions not met'` `};     }     _// Check environmental conditions_     if (!this.matchesEnvironmentalConditions(policy.environment, context)) {       return { effect:` `'DENY'``, reason:` `'Environmental conditions not met'` `};     }     return { effect: policy.effect, reason:` `'Policy matched'` `};   }   private async matchesResourceConditions(     resourceCondition: ResourceCondition,    resource: ResourceContext,     user: UserContext   ): Promise<``boolean``> {     _// This is where scope restrictions are enforced_     switch (resourceCondition.type) {       case` `'OBJECTIVE'``:         return this.evaluateObjectiveConditions(resourceCondition, resource, user);       default:         return true;     }   }   private async evaluateObjectiveConditions(     condition: ResourceCondition,     resource: ResourceContext,     user: UserContext   ): Promise<``boolean``> {     const objective = resource as ObjectiveContext;     _// Team Leader: Can only access their team's objectives_     if (user.hierarchyLevel === HierarchyLevel.TEAM_LEAD) {       const userTeamIds = user.leadsTeams.map(t => t.id);       _// Can access if objective belongs to their team_       if (objective.teamId && userTeamIds.includes(objective.teamId)) return true;       _// Can access individual objectives of team members_       if (objective.scope ===` `'individual'` `&& objective.owner.teamId &&          userTeamIds.includes(objective.owner.teamId)) return true;       return false;     }     _// Department Head: Can access department and subordinate team objectives_     if (user.hierarchyLevel === HierarchyLevel.DEPARTMENT_HEAD) {       const userDepartmentIds = user.leadsDepartments.map(d => d.id);       _// Department objectives_       if (objective.departmentId && userDepartmentIds.includes(objective.departmentId)) return true;       _// Team objectives under their department_       if (objective.team?.departmentId && userDepartmentIds.includes(objective.team.departmentId)) return true;       _// Individual objectives of department members_       if (objective.scope ===` `'individual'` `&& objective.owner.departmentId &&          userDepartmentIds.includes(objective.owner.departmentId)) return true;       return false;     }     _// CEO: Access everything_     if (user.hierarchyLevel === HierarchyLevel.CEO) return true;     return false;   } } _// Policy definitions (this is the industry ABAC pattern)_ interface AccessPolicy {   id:` `string``;   effect:` `'ALLOW'` `|` `'DENY'``;   subject: SubjectCondition;   action:` `string``[];   resource: ResourceCondition;   environment?: EnvironmentCondition; } _// Example policies_ const OBJECTIVE_POLICIES: AccessPolicy[] = [   {     id:` `'team-lead-manage-team-objectives'``,     effect:` `'ALLOW'``,     subject: {      permissions: [Permission.OBJECTIVE_MANAGE_TEAM],       hierarchyLevel: HierarchyLevel.TEAM_LEAD    },     action: [``'CREATE'``,` `'READ'``,` `'UPDATE'``,` `'DELETE'``],     resource: {      type:` `'OBJECTIVE'``,       scope: [``'team'``,` `'individual'``],       condition:` `'resource.team.id IN subject.leadsTeams[].id OR (resource.scope = "individual" AND resource.owner.team.id IN subject.leadsTeams[].id)'`     `}   },   {     id:` `'department-head-manage-department-objectives'``,     effect:` `'ALLOW'``,     subject: {      permissions: [Permission.OBJECTIVE_MANAGE_DEPARTMENT],       hierarchyLevel: HierarchyLevel.DEPARTMENT_HEAD    },     action: [``'CREATE'``,` `'READ'``,` `'UPDATE'``,` `'DELETE'``],     resource: {      type:` `'OBJECTIVE'``,       scope: [``'department'``,` `'team'``,` `'individual'``],       condition:` `'resource.department.id IN subject.leadsDepartments[].id OR resource.team.department.id IN subject.leadsDepartments[].id'`     `}   } ];`

### 3. **Guard Integration (PEP - Policy Enforcement Point)**

typescript

Copy

`_// src/shared/permissions/guards/abac.guard.ts_ @Injectable() export class AbacGuard implements CanActivate {   constructor(     private policyEngine: PolicyEngineService,     private reflector: Reflector,   ) {}   async canActivate(context: ExecutionContext): Promise<``boolean``> {     const requiredPermissions = this.reflector.getAllAndOverride<Permission[]>(       RequirePermissions,       [context.getHandler(), context.getClass()],     );     if (!requiredPermissions?.length) return true;     const request = context.switchToHttp().getRequest();     const { user, params, body } = request;     _// Build access request_     const accessRequest: AccessRequest = {       userId: user.id,       action: this.getActionFromMethod(context),       resourceType: this.getResourceType(context),       resourceId: params?.id ? parseInt(params.id) : undefined,       context: { params, body, requiredPermissions },     };     _// Evaluate with ABAC policy engine_     const decision = await this.policyEngine.evaluateAccess(accessRequest);     if (decision.effect ===` `'DENY'``) {       throw new ForbiddenException(decision.reason);     }     return decision.effect ===` `'ALLOW'``;   } }`

### 4. **Service Layer with ABAC Filtering**

typescript

Copy

`_// src/objectives/objectives.service.ts_ @Injectable() export class ObjectivesService {   async findAllForUser(userId:` `number``, filters: ObjectiveFilters = {}): Promise<Objective[]> {     _// Let ABAC policy engine determine what user can see_     const accessibleObjectives = await this.policyEngine.getAccessibleResources({       userId,       action:` `'READ'``,       resourceType:` `'OBJECTIVE'``,       filters,     });     return accessibleObjectives;   }   async create(createDto: CreateObjectiveDto, userId:` `number``): Promise<Objective> {     _// Validate creation scope through ABAC_     const canCreate = await this.policyEngine.evaluateAccess({       userId,       action:` `'CREATE'``,       resourceType:` `'OBJECTIVE'``,       context: { objectiveScope: createDto.scope, targetTeamId: createDto.teamId },     });     if (canCreate.effect !==` `'ALLOW'``) {       throw new ForbiddenException(canCreate.reason);     }     return this.createObjective(createDto, userId);   } }`

## Industry Terms Summary

- **Flat RBAC**: Explicit permissions without inheritance
- **Hierarchical RBAC**: Permissions with inheritance (convenience feature)
- **ABAC**: Context-aware access control using attributes
- **Policy Engine**: Service that evaluates access policies
- **PEP (Policy Enforcement Point)**: Guards that enforce decisions
- **PDP (Policy Decision Point)**: Engine that makes access decisions

## Benefits of This Approach

1. **Industry Standard**: Follows NIST guidelines for ABAC
2. **Flexible**: Users create custom roles with explicit permissions
3. **Convenient**: Templates provide hierarchy-based starting points
4. **Secure**: Runtime context prevents unauthorized scope access
5. **Scalable**: Easy to add new policies and conditions
6. **Auditable**: Clear policy evaluation trails

This is exactly how enterprise systems like AWS IAM, Azure RBAC, and Google Cloud IAM work - they combine explicit permissions with context-aware policy evaluation.