Version 0.3.7
-------------

New features:
 * Implement basic Windows OpenGL graphics binding.

Windows (MSVC) build fixes:
 * Session: #ifdef X11 specific workaround.
 * CMake: Require osgUtil.
 * Manager: Avoid undefined Mirror.
 * Fix Win32 DLL imports/exports.
 * Subaction: Use C++11 smart pointers for \_private to avoid undefined Private
   implementation class.
 * Fix build against old OpenGL headers.

ABI changes (ABI version 5):
 * Subaction: Use C++11 smart pointers for \_private.
 * Switch Action, ActionSet & InteractionProfile pimpls to std::unique\_ptr<>.

Version 0.3.6
-------------

Bug fixes:
 * Work around Monado GL context assumptions for xrCreateSession &
   xrCreateSwapchain calls.
 * Permit new swapchain formats: GL\_RGB10\_A2 (for Monado on AMD) & GL\_RGBA8.

Behaviour changes:
 * Report list of swapchain formats on failure to choose one.

Behind the scenes:
 * Minor whitespace cleanups in src/XRState.cpp.

Version 0.3.5
-------------

Bug fixes:
 * Ensure OpenXR::Action is valid before creating an action state object.
 * Don't suggest empty InteractionProfile bindings to OpenXR.

Behaviour changes:
 * Manager (XRState) no longer keeps counted references to ActionSets or
   InteractionProfiles. The app should manage the lifetime of these objects
   itself, and can now safely discard and recreate them.
 * All created Actions are now provided to OpenXR even if unreferenced by any
   InteractionProfile suggested bindings.
 * Action setup is now treated as a separate initialisation stage. As such if
   no action sets or no interaction profiles have been created, action setup
   will now take place on the next update() after they are created.

New/expanded APIs:
 * Manager::syncActionSet() - To inform osgXR that actions, action sets, or
   interaction profiles have been altered, so it can take action to apply them
   as soon as possible.
 * ActionPose::Location::operator !=, ActionPose::Location::operator == - For
   comparing pose location objects for equality.

Behind the scenes:
 * Some minor refactoring in src/Action.cpp.

Version 0.3.4
-------------

Bug fixes:
* Fix draw pass accounting and slave cam VR mode.

Behaviour changes:
* Automatically fall back from SceneView mode if the view configuration isn't
  stereo.

New/expanded APIs:
* New action APIs for exposing OpenXR actions (both input and haptics), action
  sets, interaction profiles, and subactions:
  * osgXR/Action: New Action, ActionBoolean, ActionFloat, ActionVector2f,
    ActionPose and ActionVibration classes to represent different kinds of
    XrAction.
  * osgXR/ActionSet: New ActionSet class to group actions that can be activated
    and deactivated together.
  * osgXR/InteractionProfile: New InteractionProfile class to allows default
    action bindings for interaction profiles to be suggested.
  * osgXR/Subaction: New Subaction class to represent a subaction path (or top
    level user path) which groups physical interactions, allowing single
    actions that apply to both hands to be handled separately.

Behind the scenes:
* Add internal action management classes in OpenXR namespace.
* Wrap XrSpace in an OpenXR::Space class.
* Wrap XrPath in an OpenXR::Path class.
* Extend inline code documentation.
* Code cleanups.

Version 0.3.3 (formerly 0.4.0)
------------------------------

New/expanded APIs:
* Settings::getVisibilityMask(), Settings::getVisibilityMask() - for setting
  whether osgXR should create visibility masks (when supported by the OpenXR
  runtime).
* Manager::hasVisibilityMaskExtension() - for finding whether the visibility
  mask extension is supported by the OpenXR runtime.
* Manager::setVisibilityMaskNodeMasks() - for setting the left and right eye
  NodeMasks to use for visibility masks.

Behind the scenes:
* Implement creation, caching, and updating of visibility mask geometry for each
  OpenXR view.
* Implement rendering of visibility masks to the depth buffer to reduce fragment
  overhead when GPU bound.

Version 0.3.2
-------------

Bug fixes:
* Fix a couple of bugs around session recreation (used when VR or swapchain mode
  changes).

Behaviour changes:
* Pick depth buffer format based on GraphicsContext traits depth bits.
* Enable depth info submission at session state to allow it to be dynamically
  switched without hitting a SteamVR hang during instance destruction.

Behind the scenes:
* Fix a few inconsequential compiler warnings with -Wall.
* Minor cosmetic cleanups (whitespace, explicit include).

Version 0.3.1
-------------

Behind the scenes:
* Fix possible crash in XRState::isRunning() if session is delayed coming up.

Version 0.3.0
-------------

API changes:
* Manager::checkAndResetStateChanged() - to detect when VR state may have
  changed, requiring app caches to be invalidated or synchronised with the new
  state.

Version 0.2.1
-------------

Behind the scenes:
* Make frame view location accessors thread safe so multiple cull threads can be
  used.

Version 0.2.0
-------------

API changes:
* Cleanups (moving dynamic bits out of Settings).
* Manager::update() - should be called regularly to allow for incremental
  bringup and OpenXR event handling.
* Manager::setEnabled() - for triggering bringing up/down of VR.

New/expanded APIs:
* Manager::destroyAndWait() and Manager::isDestroying() - for clean shutdown
  before program exit.
* Manager::syncSettings() - to trigger appropriate reinitialisation to handle
  any changed settings.
* Manager::getStateString() - to get a user readable description of the current
  VR state.
* Manager::onRunning() - virtual callback when VR has started (consider setting
  up desktop mirrors).
* Manager::onStopped() - virtual callback when VR has stopped (consider
  removing desktop mirrors).
* Manager::onFocus() - virtual callback when VR app is running in focus
  (consider resuming if paused).
* Manager::onUnfocus() - virtual callback when VR app has lost focus (consider
  pausing the experience).

Behind the scenes highlights:
* Make OpenXR bringup incremental, reversible and restartable.
* Improved handling of SteamVR's messing with GL context and threading.
* Separated event handling.

Version 0.1.0
-------------

This represents early development with the API still in heavy flux.

It supported:
* A Manager class with virtual callbacks for configuring views.
* Desktop mirrors of the VR views.
