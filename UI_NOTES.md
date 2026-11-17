# UI Handoff Notes: UgandaConnect

XML is in `app/src/main/res/`. The Java files (`MainActivity`, `SearchActivity` and four fragments)
only load layouts. All logic is for the logic team.

## Structure

| Page in the design | File |
|---|---|
| App shell, phone (top bar, bottom bar, FAB, drawer) | `layout/activity_main.xml` |
| App shell, tablet 600dp and wider (navigation rail) | `layout-sw600dp/activity_main.xml` |
| Home (4 service cards) | `layout/fragment_home.xml` |
| Solar Subsidies | `layout/fragment_solar_subsidies.xml` |
| My Results (UCE/UACE) | `layout/fragment_results.xml` and `item_subject_result.xml` |
| Healthcare (hospital list) | `layout/fragment_hospitals.xml` and `item_hospital.xml` |
| Search | `layout/activity_search.xml` (reuses `item_hospital.xml`) |
| Drawer | `nav_header.xml` and `menu/drawer_menu.xml` |

Pages are fragments shown in `navHostContainer`. `MainActivity` shows `HomeFragment` on first launch.

## View IDs

| Where | IDs |
|---|---|
| Shell (phone and tablet) | `drawerLayout`, `coordinator`, `appBarLayout`, `toolbar`, `navHostContainer`, `fab`, `navView` |
| Phone only | `bottomAppBar`, `bottomNav` |
| Tablet only | `navRail` |
| Top bar bell | `notificationsAction`, `ivBell`, `tvBadge` (the unread count text) |
| Menu item IDs (top bar) | `action_search`, `action_notifications` |
| Menu item IDs (bottom bar and rail) | `nav_home`, `nav_services`, `nav_bookmarks`, `nav_profile` (`nav_fab_slot` is the empty slot under the FAB) |
| Menu item IDs (drawer) | `drawer_home`, `drawer_energy`, `drawer_health`, `drawer_education`, `drawer_agencies` |
| Home | `cardEnergy`, `cardHealthcare`, `cardEducation`, `cardAgencies` |
| Solar Subsidies | `cardSolarSubsidy`, `tvSolarTitle`, `tvSolarDesc` |
| Results | `btnUce`, `btnUace`, `tvExamName`, `tvCandidateName`, `tvIndexNumber`, `rvSubjects` |
| Subject row | `tvSubjectName`, `tvGrade` |
| Hospital list and row | `rvHospitals`, `cardHospital`, `tvHospitalName`, `tvHospitalLocation` |
| Search | `btnBack`, `etSearch`, `btnClear`, `rvSearchResults` |

## Things to watch

1. **Null checks.** `bottomAppBar` and `bottomNav` do not exist on tablets, and `navRail` does not exist on phones. Check for null with `findViewById`.
2. **Dynamic colour.** There is one theme per service in `values/themes.xml`: `Theme.UC.Home`, `Theme.UC.Energy`, `Theme.UC.Health`, `Theme.UC.Education`.
   The top bar, FAB, selected pill, badge and status bar all follow the theme.
   Switch with `setTheme(R.style.Theme_UC_Health)` before `setContentView` (for example from an intent extra), or call `recreate()` after changing it.
3. **Collapsing top bar.** The toolbar and flag stripe use `scroll|enterAlways`.
   Page content must be a `NestedScrollView` or `RecyclerView`. All pages already are.
4. **Bottom bar and keyboard.** Both activities use `adjustResize` in the manifest.
   To hide `bottomAppBar` when the keyboard appears, use a `WindowInsets` listener.
   The search page has no bottom bar, so the design is already covered.
5. **Results selector.** `btnUce` and `btnUace` are checkable buttons. When one is checked, uncheck the other.
6. **Grade chip colours.** `tvGrade` defaults to the dark chip (black with yellow text, for D grades).
   For C grades set the background to `@drawable/bg_chip_light` and the text colour to `@color/uc_chip_light_text`.
7. **FAB.** On phones the FAB is anchored to the bottom bar, which has a cutout for it. Its colour follows the service theme.
8. **Badge.** Set `tvBadge` text from the unread count, and hide it when the count is 0.

## Other notes

- The icons are my own simple outline icons, not the designer's exact files. If the designer sends official icons, replace the files with the same names in `drawable/`.
- The font is the system font. Add the designer's font if one is specified.
- If you need a new view, ask the UI developer to add it to the XML instead of editing the layouts yourself.
