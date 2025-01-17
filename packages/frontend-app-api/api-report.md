## API Report File for "@backstage/frontend-app-api"

> Do not edit this file. It is a report generated by [API Extractor](https://api-extractor.com/).

```ts
import { Config } from '@backstage/config';
import { ConfigApi } from '@backstage/core-plugin-api';
import { ExtensionDataRef } from '@backstage/frontend-plugin-api';
import { ExternalRouteRef } from '@backstage/frontend-plugin-api';
import { FrontendFeature } from '@backstage/frontend-plugin-api';
import { IconComponent } from '@backstage/core-plugin-api';
import { JSX as JSX_2 } from 'react';
import { RouteRef } from '@backstage/frontend-plugin-api';
import { SubRouteRef } from '@backstage/frontend-plugin-api';

// @public (undocumented)
export function createApp(options?: {
  icons?: {
    [key in string]: IconComponent;
  };
  features?: (FrontendFeature | CreateAppFeatureLoader)[];
  configLoader?: () => Promise<{
    config: ConfigApi;
  }>;
  bindRoutes?(context: { bind: CreateAppRouteBinder }): void;
}): {
  createRoot(): JSX_2.Element;
};

// @public
export interface CreateAppFeatureLoader {
  getLoaderName(): string;
  load(options: { config: ConfigApi }): Promise<{
    features: FrontendFeature[];
  }>;
}

// @public
export type CreateAppRouteBinder = <
  TExternalRoutes extends {
    [name: string]: ExternalRouteRef;
  },
>(
  externalRoutes: TExternalRoutes,
  targetRoutes: PartialKeys<
    TargetRouteMap<TExternalRoutes>,
    KeysWithType<TExternalRoutes, ExternalRouteRef<any, true>>
  >,
) => void;

// @public (undocumented)
export function createExtensionTree(options: { config: Config }): ExtensionTree;

// @public
export function createSpecializedApp(options?: {
  icons?: {
    [key in string]: IconComponent;
  };
  features?: FrontendFeature[];
  config?: ConfigApi;
  bindRoutes?(context: { bind: CreateAppRouteBinder }): void;
}): {
  createRoot(): JSX_2.Element;
};

// @public (undocumented)
export interface ExtensionTree {
  // (undocumented)
  getExtension(id: string): ExtensionTreeNode | undefined;
  // (undocumented)
  getExtensionAttachments(id: string, inputName: string): ExtensionTreeNode[];
  // (undocumented)
  getRootRoutes(): JSX_2.Element[];
  // (undocumented)
  getSidebarItems(): JSX_2.Element[];
}

// @public (undocumented)
export interface ExtensionTreeNode {
  // (undocumented)
  getData<T>(ref: ExtensionDataRef<T>): T | undefined;
  // (undocumented)
  id: string;
}
```
